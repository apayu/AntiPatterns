
model:
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

view:
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

