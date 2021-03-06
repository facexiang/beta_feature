# beta_feature

[![Build Status](https://secure.travis-ci.org/helloworld1812/beta_feature.svg)](http://travis-ci.org/helloworld1812/beta_feature)

**beta_feature** is a database-based feature flag tool for Ruby on Rails, which allows you check in incomplete features without affecting users.


## Supported ORMs

beta_feature is currently ActiveRecord only.

## Supported Database

- PostgreSQL ✅
- MySQL ❌(Working in Progress)
- SQLite ❌(Working in Progress)


## Installation

Add this line to your application's Gemfile:

```ruby
gem 'beta_feature'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install beta_feature


Then, from your Rails app directory, run the following command to complete the setup.


    $ rails generate beta_feature:install
    $ rake db:migrate

Then, add your feature sets in `config/beta_features.yml`

```yml
dark_mode:
  developer: 'ryan@corp.com'
  qa: 'windy@corp.com'
  status: 'in_progress'
  description: 'Building dark mode for my website.'
```


## Usage

### Example: User Level feature toggle

Simply call `flagger` on your `User` model:

```ruby
class User < ActiveRecord::Base
  flagger

end

```

The instance of User will have the ability to toggle feature flags.


```ruby
user = User.find(327199)

#enable the feature flag dark_mode for this user.
user.enable_beta!(:dark_mode)

#remove landing_page_ux_improvement from this user's feature flags.
user.remove_beta!(:landing_page_ux_improvement)

# check whether a feature is active for a particular user.
user.can_access_beta?(:dark_mode) # => true/false
```


Controll the logic based on feature flag.

```ruby

if current_user.can_access_beta?(:dark_mode)
  render 'home', layout: 'dark'
else
  render 'home'
end
```


## Example: Company Level feature toggle

For enterprise application, you might want to enable a beta feature for all the employees of specific company. You can do it in this way.

Simply call `flagger` on your `Company` model:

```ruby
class Company < ActiveRecord::Base
  flagger

end
```

The instance of Company will have the ability to toggle feature in company level.

```ruby
company = Company.find(2)

#enable the feature flag dark_mode for this company.
user.enable_beta!(:dark_mode)

#remove the feature flag landing_page_ux_improvement from this company's feature flags.
user.remove_beta!(:landing_page_ux_improvement)

# check whether a feature is active for this company.
user.can_access_beta?(:dark_mode) # => true/false
```

Controll the logic based on feature flag.

```ruby
if company.can_access_beta?(:landing_page_ux_improvement)
  render 'landing_page_v2'
else
  render 'landing_page'
end
```

## Granularity

User level and company level beta toggles could meet most applications. But if you have special requirements, you can enable feature toggle to any models by calling `flagger`.


```ruby
class Group < ActiveRecord::Base
  flagger

end

group = Group.find(397)

#enable the feature flag dark_mode for this group.
group.enable_beta!(:dark_mode)

#remove the feature flag landing_page_ux_improvement from this group's feature flags.
group.remove_beta!(:landing_page_ux_improvement)

# check whether a feature is active for this group.
group.can_access_beta?(:dark_mode) # => true/false
```



## Alternative

- [rollout](https://github.com/fetlife/rollout), a redis based feature flags.
- [flipper](https://github.com/jnunemaker/flipper)


## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/helloworld1812/beta_feature. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

## Brought to you by

| pic | @mention | area |
|:--|:--|:--|
| ![](https://avatars2.githubusercontent.com/u/1224077?s=64) | @xiaoronglv | most things. |


## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the BetaFeature project’s codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/[USERNAME]/beta_feature/blob/master/CODE_OF_CONDUCT.md).
