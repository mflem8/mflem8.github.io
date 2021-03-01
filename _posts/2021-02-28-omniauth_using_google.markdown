---
layout: post
title:      "**OmniAuth Using Google**"
date:       2021-03-01 02:35:26 +0000
permalink:  omniauth_using_google
---


One of the requirements of the Rails Mod project was to allow login from another service such as Facebook, Twitter, Github, etc. I decided to utilize Google OAuth2. While there are many resources available, I wanted to detail my experience setting up and integrating it into my app, Digital Humidor to complete my mod 3, for Rails.

# Getting Started

The first step is to visit the Google Developers Console (https://console.developers.google.com/cloud-resource-manager) to set up your credentials. 

Click on 'Create Project' and enter the name of your applicatioin. Location/Organzation is an optional step. In the left-hand menu, hover over 'APIs & Services' and select 'Credentials'. 

*If this is a new project, you'll first need to select 'OAuth consent screen' and select User Type 'External'. Provide the App name, User support e-mail, and if you're feeling creative - a logo.*

Next you'll want to click the 'Credentials' link in the menu and select '+CREATE CREDENTIALS' and pick 'OAuth client ID'.

After selecting Web Application as your Application Type, you'll want to proviide an Authorized Redrect URI. Since we are in development mode, you can add http://localhost:3000/auth/google_oauth2/callback. 

Once you click create, you'll be provided your GOOGLE_CLIENT_ID and GOOGLE_CLIENT_SECRET.

# Next Steps
Go back to your code editor and the following gems to your Gemfile:

```
gem 'omniauth'
gem 'omniauth-google-oauth2'
gem 'dotenv-rails'
```

Bundle install.

Then create a .env file in the root directory. Here you will store your credentials from Google. You do not need quotation marks and can copy them directly from Google. Your .env file will look like this:

```
GOOGLE_CLIENT_ID= -------------------- (whatever you got from Google)
GOOGLE_CLIENT_SECRET=-------------- (again, copy this from Google)
```

**It is critical to then add .env your .gitignore file. If you do not, your credentials will be pushed to Github and accessible to anyone, totally destroyiing the security of your app.**

# Configuring
Next we need to add in some middleware to take care of our user's requests to log iin with Google. Navigate to config/initializers in your code, and add a file called **omniauth.rb**. 

In that file add the following code verbatim:

```
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :google_oauth2, ENV['GOOGLE_CLIENT_ID'], ENV['GOOGLE_CLIENT_SECRET']
end
```

# Setting up in your Application

Now that you're almost up and running -- don't forget to add a link to your views!

I have a custom view for my sessions controller to handle logins/signup, but you'll want to place a link or button wherever users go to do these things. Mine looked like this:

```
<%= link_to "Sign up / Sign in with Google", "/auth/google_oauth2" %>
```

# Route It
When a User attempts to log in with Google, a GET request will be sent back to our application with the User's info. We need to define a route to handle that request in our config/routes.rb file:

```
get '/auth/:provider/callback' => 'sessions#omniauth"
```

# Almost There...

Head over to your Sessions Controller, where we need to define an action to process the User's info received from Google to either create a new user or log in an existing user. Here I created 2 actions:

```
 def omniauth
        @user = User.create_by_google_omniauth(auth)
        session[:user_id] = @user.id
        redirect_to user_path(@user)
    end

    private

    def auth
        request.env['omniauth.auth']
    end
```

*You will need to tailor these to your specific application*.

# Last Step!
Lastly, you may have noticed in my omniauth action, I called "User.create_by_google_omniauth(auth)". What the?? That method doesn't exist yet!

In your User model, we need to write a method that will try to find our user by their uID and provider attributes. If the user can't be found, it will then create a new user. For me, that method look:

```
 def self.create_by_google_omniauth(auth)
        self.find_or_create_by(username: auth[:info][:email]) do |u|
            u.password = SecureRandom.hex
        end
    end
```

If the user is *not* found in our database, the code inside the block will run. We use SecureRandom.hex to set a random password for our user, since we (and hopefully you) previously created User validations in the model requiring a password!

And that's about it! These steps were successful for me, and I hope they are for you as well. I enjoyed the experience of setting up OAuth2 with Google and I hope this was a helpful read. Cheers!
