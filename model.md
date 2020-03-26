# Voyeuristic Models

## follow the Law of Demeter(LoD)

### model

    class Address < ApplicationRecord
      belongs_to :customer
    end

    class Customer < ApplicationRecord
      has_one :address
      has_many :invoices

      ## good
      delegate :street, :city, :state, :zip_code, to: :address

      ## bad
      # def street
      #   address.street
      # end
      #
      # def city
      #   address.city
      # end
      #
      # def state
      #   address.state
      # end
      #
      # def zip_code
      #   address.zip_code
      # end
    end

    class Invoice < ApplicationRecord
      belongs_to :customer

      ## good
      delegate :name,
               :street,
               :city,
               :state,
               :zip_code,
               to: :customer,
               prefix: true

      ## bad
      # def customer_name
      #   customer.name
      # end
      #
      # def customer_street
      #   customer.street
      # end
      #
      # def customer_city
      #   customer.city
      # end
      #
      # def customer_state
      #   customer.state
      # end
      #
      # def customer_zip_code
      #   customer.zip_code
      # end
    end

### view

bad

    <% @invoices.each do |i| %>
      <div>
        <%= i.id %>
        <%= i.customer.name %>
        <%= i.customer.address.street %>
        <%= i.customer.address.city %>
        <%= i.customer.address.state %>
        <%= i.customer.zip_code %>
      </div>
    <% end  %>

use only one dot

good

    <% @invoices.each do |i| %>
      <div>
        <%= i.id %>
        <%= i.customer_name %>
        <%= i.customer_street %>
        <%= i.customer_city %>
        <%= i.customer_state %>
        <%= i.customer_zip_code %>
      </div>
    <% end  %>

## Push All find() Calls into Finders on the Model

not good

    def self.ordered
      order("id desc")
    end

good

    scope :ordered, -> { order("id desc") }


