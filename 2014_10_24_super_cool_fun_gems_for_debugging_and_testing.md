> This was originally published in Ocotber of 2014.

One lesson learned early on in application development is that it's not just about writing your own code, it's about knowing when to use existing libraries.
For our purposes we'll define a library (a 'gem' in the wonderful world of Ruby) as simply a collection of resources (including source code) written by someone else that we can use in our own programming endeavours.

Today I thought I'd share with you a few gems that have made their way into nearly all of my Rails projects.  The first two are helpful for debugging, and the final two are for testing.
In order of appearance:

* [pry](https://github.com/pry/pry)
* [better errors](https://github.com/charliesome/better_errors)
* [rspec](https://www.relishapp.com/rspec)
* [capybara](https://github.com/jnicklas/capybara)

# Pry

From github, Pry is "An IRB alternative and runtime developer console".  To elaborate, pry does two main things.  First, as an 'IRB alternative' it provides a lot of added functionality, most exciting of which for me was syntax highlighting.  It does other stuff too like easy documentation and source code browsing, and  (which is cool too I guess).  For example in pry you can type `$ Array#each` to browse the C source for that particular method (`? Array#each` will bring you to the documentation).

Pry also has an incredibly useful feature that let's you jump into your code at any point and poke around.  As a beginner, learn to love this.  It has helped debug so, so many errors for me and it gives you a great idea of what your code is really doing.  The short version: plop `binding.pry` into your code where you want pry to hook in, run your code, poke around in the console pry makes
available to you.  Super easy, incredibly useful.

The console that opens will have access to all of the variables that were available in that particular scope.  You can go in and see if your variables contain the values you expect them to, or you can try out new code and get immediate feedback about the results.  Since we're talking about Rails, here's a simple example of the result of dropping binding.pry in your code.  Say we have an articles resource ([Blogger](http://tutorials.jumpstartlab.com/projects/blogger.html) shoutout) and a show action we wanted to investigate. We'd do the following:

```ruby
def show
  @article = Article.find(params[:id])
  binding.pry # where the magic happens
end
```

When we trigger that action the pry console should drop us right into the code with access to the value of the `@article` instance variable at that point in the program's execution.

![pry](https://i.imgur.com/4cBqglR.png)

## Better Errors

The Rails error page is something you'll get used to seeing a lot, and it's much more helpful with Better Errors.  Here is an example error screen:

![better errors](https://camo.githubusercontent.com/3fa6840d5e20236b4f768d6ed4b42421ba7c2f21/68747470733a2f2f692e696d6775722e636f6d2f367a42474141622e706e67)

I'd 100% recommend adding [binding of caller](https://github.com/banister/binding_of_caller) in addition to Better Errors, as it grants you access to 'advanced' features, such as access to the REPL and local/instance variable inspection.  I really can't emphasize enough how valuable of a tool it is to be able to poke around your code with all of your variables still in scope.

## RSpec

RSpec is a ruby library for testing that encourages human readability (Check out [this](http://rspec.info/) to get started).  I'll leave it up to you to decide which testing frameworks you like best, but know that RSpec is popular in large part because of the kind of syntax it defines.  I'd recommend browsing some RSpec source to see if you find it particularly readable, if you enjoy speaking RSpec's 'language' then you might consider using it in your own projects.

Wiring RSpec in rails the first few times can be a little hairy (I'm not prepared to admit how often I forget to tack `--skip-test-unit` on `rails new`).  From the beginning:

- Add the `--skip-test-unit` option when creating a new rails project.

`$ rails new my_app --skip-test-unit`

- Add `rspec-rails` to your Gemfile under both the development and test group
(for more on bundler groups see [here](http://yehudakatz.com/2010/05/09/the-how-and-why-of-bundler-groups/)).

```ruby
group :development, :test do
  gem 'rspec-rails'
end
```

- From the command line run `$ bundle install`.

- Run `$ rails generate rspec:install`

This should have created a `spec/` directory as well as some helper files to get you started.

## Capybara

Capybara is a tool for writing acceptance tests which integrates really nicely with Rails and RSpec.
Acceptance tests are great because they mimic the way an end-user would experience your application.
They're great for documentation, exercise your entire stack, and save valuable time that may have been spent doing manual testing.
Mission critical features like the login process are usually a good fit for Capybara.  A very basic login spec might look something like this:

```ruby
describe "login", type: :feature do

  describe "user with valid credentials" do
    it "can log in successfully" do
      visit "/"
      click_link "Log in"
      fill_in "Email", with: "jonsnow@whitewall.gov"
      fill_in "Password", with: "kn0wsn0th1ng"
      click_button "Submit"

      expect(page).to have_content "Logged in as Jon Snow"
    end
  end
end

```

Here we visited our applications root url, clicked 'Log in', filled in our information and clicked "Submit", and finally asserted that some content appeared alerting us the login process was successful. As you can see the explanation probably wasn't necessary given how supremely readable tests with Capybara can be.

Basic setup:

- As usual add `gem 'capybara'` to your Gemfile
- Drop `require 'capybara/rails'` and `require 'capybara/rspec'` into your `spec_helper.rb`

#### Launchy

[Launchy](https://github.com/copiousfreetime/launchy) deserves a special mention for it's
lovely interaction with capybara's `save_and_open_page`.  All you have to do is add `gem 'launchy'` to your gemfile and a browser tab will be opened when `save_and_open_page`
is executed.

And that's it!  Hopefully you'll find some use out of these gems, and as always don't forget to thank your local open source maintainers and contributors!



