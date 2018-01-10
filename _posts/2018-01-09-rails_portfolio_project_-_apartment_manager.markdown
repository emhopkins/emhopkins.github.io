---
layout: post
title:      "Rails Portfolio Project - Apartment Manager"
date:       2018-01-10 01:08:57 +0000
permalink:  rails_portfolio_project_-_apartment_manager
---


For my rails portfolio project, I build an Apartment Manager app (can be found [here](https://github.com/emhopkins/rails-portfolio-project)).  My original goal was to build an app with multiple user roles: landlord and tenant.  Then I would build different sections of the app available to each users's role.  As I began implementing the landlord fuctionality, I realized that the tenant half the app wasn't needed at this time, but I've left the tenant model and associations within the app in case I chose to expand it later.  

The basic flow of my application is:

1. A new user signs up.  The authentication is handled through the [Devise gem](https://github.com/plataformatec/devise).  The user has the option to sign up for an account with the app or login with Facebook. 
2. A user selects their role (at this time the only option is landlord). 
3. The landlord then can begin adding buildings.  
4. Once a building is added and the number of apartments is saved, the user can create apartments that belong to the building. 

I am using the following models in my application:

1. User, as well as the fields needed for Devise and Omniauth with Facebook, I also added a role column to User.  The role is an enum that can be defined as either landlord or tenant:
```
 enum role: [:landlord, :tenant]
```

2. Landlord - the landlord class belongs to a user and has many buildings, apartments and tenants.  
```
class Landlord < ApplicationRecord
	belongs_to :user
	has_many :buildings
	has_many :apartments, through: :buildings
	has_many :tenants, through: :apartments
end
```

3. Building - the building class belongs to a landlord and has many apartments.  It is also a nested resource under landlord and accepts attributes for apartments. 
```
class Building < ApplicationRecord
	belongs_to :landlord
	has_many :apartments
	accepts_nested_attributes_for :apartments
end	
```

4. Apartment - the apartment class belongs to a building.  I created my join table between apartments and characteristics.  The apartment class has many apartment_characteristics and many characteristics through apartment characteristics. 
```
class Apartment < ApplicationRecord
	belongs_to :building
	has_many :tenants
	has_many :apartment_characteristics
	has_many :characteristics, through: :apartment_characteristics
end
```

5. ApartmentCharacteristic - this is my join model to store the ids of Apartments and Characteristics. 
```
class ApartmentCharacteristic < ApplicationRecord
	belongs_to :apartment  
	belongs_to :characteristic
end
```

6. Characteristic - this is a model that contains a unique list of characteristics that can be attributed to an apartment through the join model, ApartmentCharacteristic. 
```
class Characteristic < ApplicationRecord
	has_many :apartment_characteristics
	has_many :apartments, through: :apartment_characteristics
end
```

7. I also created a Tenant model, but I did not build it out completely. 

One of the things I stuggled with when building this application was how to allow the user to define the number of apartment forms that would appear when creating a building.  I ended up handling that in my building controller.  After the building is created with the create action, the user is redirected to the show, where they can edit the building's attributes and add/edit apartments.  I decided that I wouldn't allow the user to add apartments until they had defined a valid building with a number_of_apartments.  Then, using the number_of_apartments field as compared with the actual building.apartments.size, I was able to dynamically build the number of apartment forms needed for each building.  

Another challenging part of this application, was my nested form for the building.  This form accepted attributes for the apartments as well.  
```
<%= form_for @building, url: @url do |f| %>
	Name: <%= f.text_field :name %><br /><br />
	Address: <%= f.text_field :address %><br /><br />
	Number of Apartments: <%= f.text_field :number_of_apartments %><br /><br />
	<h4>Apartments:</h4>
	<%= f.fields_for :apartments, @building.apartments do |a| %>
		Unit: <%= a.text_field :unit %><br /><br /> 
		Max Occupants: <%= a.text_field :max_occupants %><br /><br /> 
		Rent: <%= a.text_field :rent %><br /><br /> 
		Description: <%= a.text_field :description %><br /><br /> 
		<%= a.collection_check_boxes :characteristic_ids, Characteristic.all, :id, :name %>
		<%= a.fields_for :characteristics, a.object.characteristics.build do |characteristics_fields| %>
    		<%= characteristics_fields.text_field :name %>
    	<% end %> 
		<br /> <br /> 
		<h3>----------------------------------------------------------------------------------------</h3>
 	<% end %> 
    <%= f.submit %>
<% end %>
```

The interesting part here was when adding the fields_for :characteristics in the already nested fields_for :apartments.  I struggled with finding how I could reference the specific apartment object in order to build the characteristics fields from the apartment.  After a bit of googling, I found that I could get the apartment object by the object method on the ActionView::Helpers::FormBuilder.  So, I was able to build my characteristics using `a.object.characteristics.build` .  Woo!


