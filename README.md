### 290.


> bundle install  
rails generate devise:install  
rails generate devise User  
migration
  uncoment confirmable  

**User.rb**  
:confirmable  

rails db:migrate  

**appController**  
before-action :autheticate-user! 
 
**welcome_controller**  
skip-before-action :autheticate-user!, only: [:index]  

> rails g devise:views:bootstrap_templates  
commit -m "add devise and styling."

### 291. Send email.
heroku addons:create sendgrid:starter
heroku. account settings. set credit card.
open app.
click add.
settings. api key.
heroku config:set SENDGRI_USERNAME=apikey SENDGRI_PASSWORD=apikey
.profile
config/enviroment
  ActionMailer::Base.smtp_settings = {
    :address => 'smtp.sendgrid.net',
    :port => '587',
    :authentiation => :plain,
    :user_name => ENV['SENDGRI_USERNAME'],
    :password => ENV['SENDGRI_PASSWORD],
    :domain => 'heroku.com',
    :enable_starttls_auto => true
  }

config/enviroments/dev
  config.action_mailer.delivery_method = :test
  config.action_mailer.default_url_options = { :host => 'localhost'}
production
    config.action_mailer.delivery_method = :smtp
  config.action_mailer.default_url_options = { :host => 'herokuapp', :protocol => 'https' }
teste sign up.
take link from server.
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

### 294
app.html
  navbar
    add code for if current_user edit profile, logout else sign in.
    fa_icon user, email, btn logout or sign in
commit -m "setup email functionality for sign up"
heroku master.
heroku run rails db:migrate
config/init/devise.rb
  mailer_sender = ""

### 296 - Build Homepage.
index.html
  take code.
create image to jumbotron.
back-image
back-size: cover
improve layout.
Stripe.com

### 298 - Payment Methods.
Stripe. Create account
API Keys.
Test Secret.
gem devise.
Gemfile
  gem 'stripe'
bundle install
config/init/stripe.rb
  Rails.configuration.stripe = {
    :publishable_key => ENV['STRIPE_TEST_PUBLISHABLE_KEY'],
    :secret_key => ENV['STRIPE_TEST_SECRET_KEY']
  }

  Stripe.api_key = Rails.configuration.stripe[:secret_key]
bashrc
  export STRIPE_TEST_SECRET_KEY="SECRET_KEY"
  export STRIPE_TEST_PUBLISHABLE_KEY="Publishable"
> 
heroku config:set STRIPE_TEST_SECRET_KEY="SECRET_KEY"
heroku config:set STRIPE_TEST_PUBLISHABLE_KEY="publishable_KEY"
close terminal

### 300. Payment Model.
rails g model Payment email:string token:string user_id:integer
migration
  rails db:migrate
User.rb
  has_one :payment
  accepts_nested_attributes_for :payment
Payment.rb
  attr_accessor :card_number, :card_cvv, :card_expires_month, :card_expires_year
  belongs_to :user
  
  def self.month_options
    Date::MONTHNAMES.compact.each_with_index.map{|name, i| ["#{i+1} - #{name}", i+1]}
  
  def self.year_options
    (Date.today.year..(Date.today.year+10)).to_a

  def process_payment
    customer = Stripe::Customer.create email: email, card:token

    Stripe::Charge.create customer: customer.id,
      amount: 1000,
      desc: "Premium,
      currency: 'usd'

### 302. Update Form for Payment Options.
devies/regis/new
  after password
  <%= fields_for( :payment) do |p| >
    .row.md-12>.form-group.col-md-4.pl-9
      <%= p.label :card_number, "Card number, data { stripe: 'label}>
      p.text_field :card_number, class: "form-control, rquired: true, data {stripe: number}
    .form-group.col-md-2
      <%= p.label :card_cvv, "Card CVV", data { stripe: 'label}>
      p.text_field :card_cvv, class: "form-control, rquired: true, data {stripe: cvc}
    .form-group.col-md-6>.col-md-12
        p.label :card_expires, "Card Expires", data: {stripe: label}
      .col-md-3
        p.select :card_expires_month, options_for_select(Payment.month_options),
          { include_blank: Month},
          "data-stripe", => "exp-month",
          class: form-control, required: true
      .col-md-3
        p.select :card_expires_year, options_for_select(Payment.year_options.push),
          { include_blank: Year},
          data: { stripe: "exp-year"},
          class: form-control, required: true
      /d
      /d
      /d
  <% end>

### 304. JS Events.
app.html.erb
before include tag
  include_tag https://js.stripe.com/v2/

devies/regis/new
  first line
  script language="Javascript>
    Stripe.setPublishableKey("<%= ENV['STRIPE_TEST_PUBLISHABLE_KEY']")
  form_for, class: "cc_form" }

js/credit_card_form.js
  $(document).on('ready turbolinks:load', function() {
    var show_error, stripeResponseHandler, submitHandler/

    $(".cc_form").on("submit", submitHandler);
  })