# Build a database on your local pc using ridgepole.
Ridgepole is a tool to manage DB schema.

It defines DB schema using Rails DSL, and updates DB schema according to DSL. (like Chef/Puppet)

- ridgepole : 
https://github.com/winebarrel/ridgepole

### Pre-requists
Install ruby if it is not  installed on your PC

#### Windows
Download ruby installer from the below URL and installed it accordingly.

https://rubyinstaller.org/downloads/

#### macOS
Install ruby macOS + Homebrew
```
brew update
brew install ruby
```

### Setting files
Schemafile: schema settings (main file).
database.yml: DB connection information for every environment.

- check the github for documentation

### Usage
#### Windows
- Download mysql client
- Rest is follow Mac section

#### Mac
Install ruby if not already installed
```bash
# ---------------------
# Install ruby
# ---------------------
brew update
brew install ruby

# ---------------------
# Gemfile
# ---------------------
```

##### create a Gemfile file ( no extention )
```
vi Gemfile << 

source 'https://rubygems.org'
 
gem 'ridgepole'
gem 'mysql2'
```

##### Bundle install
```
# install bundler
gem install bundler
 
# bundle install
bundle install --path .bundle
```

##### Create a database.yml that contains all dev settings;
Ex:
```sql
default: &default
  adapter: mysql2
  encoding: utf8
  database: {your_db_name}
  username: {db_user_name}

dev:
  <<: *default
  host: devserver.dev.local
  port: 3301
  password: {{ mysql.password }}

stg:
  <<: *default
  host: stgserver.stg.local
  port: 3301
  password: {{ mysql.password }}

prod:
  <<: *default
  host: prodserver.prod.local
  port: 3301
  password: {{ mysql.password }}

local:
  <<: *default
  username: root
  database: test
```
##### Schemafile - no extension
Ex:
```sql

create_table "articles", force: :cascade do |t|
  t.string   "title"
  t.text     "desc", renamed_from: "text"
  t.text     "author"
  t.datetime "created_at"
  t.datetime "updated_at"
end
add_index :articles, [:id], unique: true

create_table "user_comments", force: :cascade, renamed_from: "comments" do |t|
  t.string   "commenter"
  t.text     "body"
  t.integer  "article_id"
  t.datetime "created_at"
  t.datetime "updated_at"
end
add_index :articles, [:article_id], unique: true

```
##### Create db in mysql
Ex:
```sql
$> mysql -u root

SHOW DATABASES;
CREATE DATABASE `test`;

SHOW DATABASES;
```
##### run ridgepole
```
export environment=local # for local machine environment

# Check how the contents of Schemafile works using dryrun
bundle exec ridgepole -c database.yml -E $environment -f Schemafile --apply --dry-run

# The contents of Schemafile is executed
bundle exec ridgepole -c database.yml -E $environment -f Schemafile --apply
```

