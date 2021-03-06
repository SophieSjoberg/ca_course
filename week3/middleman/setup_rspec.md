---
title: "Middleman"
subtitle: "Setup Rspec"
author: [Craft Academy - Coding as a Craft]
date: Version 0.1
subject: "Middleman, Rspec"
keywords: [Middleman, Rspec]
titlepage: true
titlepage-color: f28e24
titlepage-text-color: "FFFFFF"
titlepage-rule-color: "FFFFFF"
titlepage-rule-height: 2
...

The following assumes that you've generated a middleman project using its CLI middleman `init your_project_name`

### Update your Gemfile
Add the following gems to your Gemfile and run `bundle install`
```ruby
group :development, :test do
  gem 'capybara'
  gem 'pry'
  gem 'pry-byebug'
  gem 'rspec'
end
```

### Setup Rspec
From your terminal, navigate to your project directory and initialize RSpec config files for your project with the following command

`rspec --init`

Update the `.rspec` file to have the following code

```shell
--format documentation
--color
--require spec_helper
```
Then remove all content in spec/spec_helper.rb and replace with the following

```ruby
require 'rspec'
require 'capybara/rspec'

require 'middleman-core'
require 'middleman-core/rack'
require 'middleman-autoprefixer'
require 'middleman-livereload'

middleman_app = ::Middleman::Application.new

Capybara.app = ::Middleman::Rack.new(middleman_app).to_app do
  set :root, File.expand_path(File.join(File.dirname(FILE), '..'))
  set :environment, :development
  set :show_exceptions, false
end
```
With this in place we should now be ready to write our first spec

### Feature specs
Most of the specs you'll be writing for a middleman app will be feature specs that test for certain things to be visible on the page. Below is a sample feature spec that checks if projects are listed on the index page

```ruby
describe 'Index Page', type: :feature do
  it 'displays project list' do
    visit '/'

    expect(page).to have_css '.projects'

    within '.projects' do
      expect(page).to have_content 'My First Website'
      expect(page).to have_content 'FizzBuzz'
    end
  end
end
```
