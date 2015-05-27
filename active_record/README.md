This folder structure should be suitable for starting a project that uses a database:

* Fork this repo
* Clone this repo
* `rake generate:migration <NAME>` to create a migration (Don't include the `<` `>` in your name, it should also start with a capital)
* `rake db:migrate` to run the migration and update the database
* Create models in lib that subclass `ActiveRecord::Base`
* ... ?
* Profit


## Rundown

```
.
├── Gemfile             # Details which gems are required by the project
├── README.md           # This file
├── Rakefile            # Defines `rake generate:migration` and `db:migrate`
├── config
│   └── database.yml    # Defines the database config (e.g. name of file)
├── console.rb          # `ruby console.rb` starts `pry` with models loaded
├── db
│   ├── dev.sqlite3     # Default location of the database file
│   ├── migrate         # Folder containing generated migrations
│   └── setup.rb        # `require`ing this file sets up the db connection
└── lib                 # Your ruby code (models, etc.) should go here
    └── all.rb          # Require this file to auto-require _all_ `.rb` files in `lib`
```

# NORMAL MODE



## How many users are there?
  ```ruby
  User.count
  ```
    50

## What are the 5 most expensive items?
```ruby
Item.order(price: :desc).limit(5)
```
    "Small Cotton Glvoes"
    "Small Wooden Computer"
    "Awesome Granite Pants"
    "Sleek Wooden Hat"
    "Ergonomic Steel Car"

## What’s the cheapest book?
```ruby
Item.where("category LIKE ?", "%book%").order(price: :asc).limit(1)
```
    "Ergonomic Granite Chair"

## Who lives at “6439 Zetta Hills, Willmouth, WY”? Do they have another address?
```ruby
Address.find_by(street: '6439 Zetta Hills')
```
    id: 43, user_id: 40, street: "6439 Zetta Hills", city: "Willmouth", state: "WY", zip: 15029
```ruby
User.find_by(id: 40)
```
    id: 40, first_name: "Corrine", last_name: "Little", email: "rubie_kovacek@grimes.net"
    No additional addresses found for user_id: 40.

## Correct Virginie Mitchell’s address to “New York, NY, 10108”.
```ruby
User.find_by(first_name: 'Virginie')
```
    id: 39, first_name: "Virginie", last_name: "Mitchell", email: "daisy.crist@altenwerthmonahan.biz"
```ruby
Address.find_by(user_id: 39).update(city: 'New York', state: 'NY', zip: 10108)
```
    id: 41, user_id: 39, street: "12263 Jake Crossing", city: "New York", state: "NY", zip: 10108

## How much would it cost to buy one of each tool?
```ruby
Item.where("category LIKE?", "%tool%").sum("price")
```
    $46,477

## How many total items did we sell?
```ruby
Order.sum("quantity")
```
    2125

## How much was spent on books?
```ruby
Order.joins('LEFT OUTER JOIN items ON orders.item_id = items.id')
     .where("category LIKE ?", '%book%').sum('price')
```
    $188,356

## Simulate buying an item by inserting a User for yourself and an Order for that User.
```ruby
User.create(first_name: "Ben", last_name: "Hand", email: "gneeshot@gmail.com")
```
```ruby
Order.create(user_id: 51, item_id: 32, quantity: 10, created_at:"")
```
