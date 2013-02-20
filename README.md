## Padrino SasS Demo

This demo application shows how Padrino can be used to divide an application into multiple small applications all running on the one server

The advantage of structuring an application this way is that it results in a very clear separation of concerns between the different aspects of your application. As an example this demo contains 3 small applications, 1 for the sales site, 1 for the core application and 1 for user login and logout.

Here's a description of each of the separate applcations.

### Padrino SaaS Website

This is the base application and stores all the shared assets and resources used throughout the entire application. Once deployed this will most probably contain your sales site or whatever content you want display by default on your domain. The advantage of having a small application behind your sales site is that you can build a lightweight CMS or perform some A/B testing which you couldn't do with a purely static website.

Here we also define which sub applications we want to mount and where they should mounted to. Inside config/apps.rb we see the following code which mounts all the applications.

    Padrino.mount("PadrinoSaasWebsite").to('/')
    Padrino.mount("PadrinoSaasApplication", app_file: Padrino.root("padrino_saas_application/app/app.rb")).to('/app')
    Padrino.mount("PadrinoSaasUsers", app_file: Padrino.root("padrino_saas_users/app/app.rb")).to('/users')

The default layout that's shared across all applications is also within this application and can be found in `app/views/layouts/website.haml`. Finally there are shared assets such as stylesheets which are in `app/stylesheets`.

### Padrino SaaS Application

At the center of your SaaS there will generally be a core application which is where your users will actually use your product. If you where building Gmail it would be the Inbox page, for twitter it would the timeline.

This application has the option to use layouts and assets from the base Website application. At the moment you need to do the following in order to use the layout from the parent application

    get '/' do
      render :index, layout: :"../../../../app/views/layouts/website"
    end

As you can see it's not an ideal way to reference the parent layout but I'm led to believe this will be tidied up in the not too distant future.

### Padrino SaaS Users

Finally to demonstrate sharing sessions across the application I've created a small users application that allows you to login and logout. You will be able to see the global navigation change to reflect your current state.
