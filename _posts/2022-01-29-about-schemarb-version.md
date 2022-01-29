---
layout: post
title: "About db:migrate and schema.rb version"
tag: [ruby, rails]
---

This week, I had an argument (a healthy one 😗) at work about the version in Rails' schema.rb.

In my feature branch, I added a migration file with version older than the latest version in main branch, so when I run `db:migrate`, the version in schema.rb is not updated. My colleage insists that anytime you commit a new migration file, the schema.rb version needs to be updated too, or `rails db:migrate` will not run that migration file in other environments. However, as I understand `rails db:migrate` does not care about the version in schema.rb, it only compares the version list in `db/migrate` folder and version list in `schema_migrations` table, and run any migration that has not been run.

We didn't have much time to talk about it considering that it's not really important, so I just change the migration file name, run `db:migrate` again in my local to update the version and commit it. Now, out of curiosity, I want to look more into this matter.

## Rails' official guide

The first place to look is always [the official guide](https://guides.rubyonrails.org/active_record_migrations.html#what-are-schema-files-for-questionmark).

> By default, Rails generates db/schema.rb which attempts to capture the current state of your database schema.

> It tends to be faster and less error prone to create a new instance of your application's database by loading the schema file via bin/rails db:schema:load than it is to replay the entire migration history. Old migrations may fail to apply correctly if those migrations use changing external dependencies or rely on application code which evolves separately from your migrations.

And [here](https://guides.rubyonrails.org/active_record_migrations.html#running-migrations).

> Note that running the db:migrate command also invokes the db:schema:dump command, which will update your db/schema.rb file to match the structure of your database.

It's just like what I see about schema.rb: just a file to store current table structure of your database. `db:migrate` will generate or modify it, but it should not affect how `db:migrate` run.


## Stackoverflow

If answers and replies in [this question](https://stackoverflow.com/questions/2979059/rails-is-the-version-number-in-schema-rb-used-for-anything) are correct, it seems like `db:migrate` used to assume that all migrations up to the version in schema.rb are applied, and will not run new migration files with older version. However, that has been already changed, and now `db:migrate` does not decide which file should be run based on that version anymore.

## Source code

> Truth can be found in one place: the code.
> ー Robert C. Martin, Clean Code

OK, enough for documents and answers. Now I need to see the truth. After searching through activerecord's source code, I confirmed my thoght: `db:migrate` will check `schema_migrations` table to determine which migration files have been run and which not. Then it will run all files of versions which are not in `schema_migrations`

[Source 1](https://github.com/rails/rails/blob/main/activerecord/lib/active_record/migration.rb#L1307)

```ruby
def migrated
  @migrated_versions || load_migrated
end

def load_migrated
  @migrated_versions = Set.new(@schema_migration.all_versions.map(&:to_i))
end
```

[Source 2](https://github.com/rails/rails/blob/main/activerecord/lib/active_record/migration.rb#L1343)

```ruby
def ran?(migration)
  migrated.include?(migration.version.to_i)
end
```

[Source 3](https://github.com/rails/rails/blob/main/activerecord/lib/active_record/migration.rb#L1287)

```ruby
def runnable
  runnable = migrations[start..finish]
  if up?
    runnable.reject { |m| ran?(m) }
  else
    # skip the last migration if we're headed down, but not ALL the way down
    runnable.pop if target
    runnable.find_all { |m| ran?(m) }
  end
end
```

## Test

Of course, we cannot have a firm conclusion if we don't test it. So here is how I test it:

1. Create a migration file with timestamp `20220129000000` in branch `main` and migrate it and commit schema.rb
2. Create a new branch `test` from `main`, add a migration file with timestamp `20220128000000` and migrate it **=> schema.rb version is expected not to change**
3. Change back to `main` branch, drop db and migrate again
4. Merge `test` into `main` and run migrate again **=> although schema.rb version does not change, the migration file `20220128000000` is expected to be applied**

### Result

After step 2, schema.rb version does not change:
![step 2](/assets/images/step2.png)

After step 3, only the migration file of version `20220129000000`
![step 3](/assets/images/step3.png)

After step 4, the migration file of verion `20220128000000` is applied even when schema.rb version does not change:
![step 4](/assets/images/step4.png)

Phew... Finally I got something to show my colleage the next time we talk! 🧐
