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