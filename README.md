# OmniAuth Line (Forked Version)
This is a fork of the original [omniauth-line](https://github.com/kazasiki/omniauth-line) gem, modified to support additional dynamic parameters for authentication.

This gem contains the Line OAuth2 Strategy for OmniAuth.
Supports the OpenID Connect Web Login. Read the Line developers docs for more details: https://developers.line.me/en/docs/line-login/web/integrate-line-login/


Modifications
- Added support for scope and bot_prompt parameters, allowing more flexibility in permission requests and bot prompts during authentication.
- Updated examples and documentation to illustrate usage of these new parameters.

## Using This Strategy

First start by adding this gem to your Gemfile:

```ruby
gem 'omniauth-line', github: 'crzbread/omniauth-line', branch: 'master'
```

Next, tell OmniAuth about this provider. For a Rails app, your `config/initializers/omniauth.rb` file should look like this:

```ruby
# PROFILE permission required!!
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :line, "Channel_ID", "Channel_Secret"
end
```

### New Features
This gem now supports dynamically setting additional parameters for LINE authentication, specifically:

- scope: Define the permissions requested during authentication. Default is "profile openid".
- bot_prompt: Configure whether to prompt users to add a bot as a friend. Set to "normal" or "aggressive".
For example, you can configure the scope and bot_prompt in your OmniAuth initializer as follows:

```ruby
# if use devise, config/initializers/devise.rb
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :line, "Channel_ID", "Channel_Secret",
  scope: "profile openid email",
  bot_prompt: "normal"
end
```
### ⚠️ Important Note
Testing for the new scope and bot_prompt parameters is currently not fully implemented.



## Authentication Hash
An example auth hash available in `request.env['omniauth.auth']`:

```ruby
{
  :provider => "line",
  :uid => "a123b4....",
  :info => {
    :name => "yamada tarou",
    :image => "http://dl.profile.line.naver.jp/xxxxx",
    :description => "breakfast now.",
  },
  :credentials => {
    :token => "a1b2c3d4...", # The OAuth 2.0 access token
    :secret => "abcdef1234"
  },
  :extra => {
    # nil
  }
}
```

## Supported Rubies

OmniAuth Line is tested under 2.1.x, 2.2.x.
