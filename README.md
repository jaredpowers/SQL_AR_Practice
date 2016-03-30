1. How many users are there?
`User.count`
  There are 50 users

2. What are the 5 most expensive items?
`Item.order(price: :desc).limit(5)`
  Small Cotton Gloves, Small Wooden Computer, Awesome Granite Pants, Sleek Wooden Hat, Ergonomic Steel Car

3. What's the cheapest book?
`Item.where("category LIKE ?", "%book%").order(price: :asc).limit(1)`
  title: "Ergonomic Granite Chair",

4. Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?
Used `Address.where(street: "6439 Zetta Hills")` to find the user (40) at the street.

Cross checked that with the users table with
`User.where(id: 40)`
Which yielded the following:
  id: 40,
  first_name: "Corrine",
  last_name: "Little",
  email: "rubie_kovacek@grimes.net">

Checked addresses with that user_id with
`Address.where(user_id: 40)`
Which yielded two addresses.

5. Correct Virginie Mitchell's address to "New York, NY, 10108".
First I found Virginie's ID with
`User.where(first_name: "Virginie")`
Which yielded
  id: 39,
  first_name: "Virginie",
  last_name: "Mitchell",
  email: "daisy.crist@altenwerthmonahan.biz"

I then found her addresses using
`Address.where(user_id: 39)`
Which yielded:
  id: 41,
  user_id: 39,
  street: "12263 Jake Crossing",
  city: "Roxanehaven",
  state: "NY",
  zip: 34705>,
and
  id: 42,
  user_id: 39,
  street: "83221 Mafalda Canyon",
  city: "Bahringerland",
  state: "WY",
  zip: 24028

I then changed the address using
`Address.where(user_id: 39).update(41, city: "New York", zip: 10108)`
Which spit out the changed address in:
  id: 41,
  user_id: 39,
  street: "12263 Jake Crossing",
  city: "New York",
  state: "NY",
  zip: 10108


6. How much would it cost to buy one of each tool?
I specifically left out categories other than just Tools with
`Item.where("category LIKE ?", "Tools").sum(:price)`
Which yielded
  7383

7. How many total items did we sell
I simply ran
`Order.sum(:quantity)`
And got
  2125

8. How much was spent on books?
Again, I specifically left out categories other than just Books with a join:
`Order.joins("LEFT OUTER JOIN items ON items.id = orders.item_id").where("category LIKE ?", "Books").sum("quantity * price")`
That gave me:
  420566

9. Simulate buying an item by inserting a User for yourself and an Order for that User.
First I created a user with:
`User.create(first_name: "Keanu", last_name: "Reaves", email: "theone@thematrix.mtrx")`
Something in the Matrix has changed:
  id: 51,
  first_name: "Keanu",
  last_name: "Reaves",
  email: "theone@thematrix.mtrx"

I then placed an order with:
`Order.new(user_id: 51, item_id: 1, quantity: 1,
[27] pry(main)* created_at: Time.now)`
Which gave me:
  id: nil,
 user_id: 51,
 item_id: 1,
 quantity: 1,
 created_at: Wed, 30 Mar 2016 05:06:10 UTC +00:00
