# rofi-pass

A bash script to handle [Simple Password Store](http://www.passwordstore.org/)
in a convenient way using [rofi](https://github.com/DaveDavenport/rofi).

![rofi-pass](https://53280.de/rofi/rofi-pass.png "rofi-pass in action")

## Features

* Open URLs of entries with hotkey
* Add new Entries to Password Storage
* Edit existing Entries
* Generate new passwords for entries
* Inline view, which can copy/type individual entries
* Move/Delete existing entries
* Support for different password stores (roots), e.g. to separate passwords for
  work from private passwords
* Type any field from entry
* Auto-typing of user and/or password fields.
  The format for password files should look like:

  ```
  foobarmysecurepassword
  user: MyUser
  url: http://my.url.foo
  ```

* Auto-typing username based on path.
  The structure of your password store must be like:

  ```
  foo/bar/site.com/username
  ```

  And you must set the `default-autotype` to `'path :tab pass'`.

* Auto-typing of more than one field, using the `autotype` entry:

  ```
  foobarmysecurepassword
  ---
  user: MyUser
  SomeField: foobar
  AnotherField: barfoo
  url: http://my.url.foo
  autotype: SomeField :tab user :tab AnotherField :tab pass
  ```

  You can use `:tab`, `:enter`, or `:space` here to type <kbd>Tab</kbd>,
  <kbd>Enter</kbd>, or <kbd>Space</kbd> (useful for toggling checkboxes)
  respectively.
  `:otp` will generate an OTP, either `pass-otp(1)` style, or according to the
  `otp_method:`, if it is defined.
* Generating OTPs.
  The format for OTPs should either compatible with `pass-otp(1)`:

  ```
  [...]
  otpauth://[...]
  ```

  Or it should define a method for generating OTPs:

  ```
  [...]
  otp_method: /opt/obscure-otp-generator/oog --some-option some args
  ```

  `:delay` will trigger a delay (2 seconds by default).
* All hotkeys are configurable in the config file
* The field names for `user`, `url` and `autotype` are configurable
* Bookmarks mode (open stored URLs in browser, default: Alt+x)
* Share common used passwords between several entries (with different URLs, usernames etc)

## Requirements

* [pass](http://www.passwordstore.org/)
* sed
* [rofi](https://github.com/DaveDavenport/rofi)
* xdotool
* gawk
* bash 4.x
* find
* pwgen
* [pass-otp](https://github.com/tadfisher/pass-otp) (optional: for OTPs)

### BSD

* gnugrep
* gawk

## Configuration

rofi-pass may read its configuration values from different locations in the following order:
* `ROFI_PASS_CONFIG` (environment variable)
* `$HOME/.config/rofi-pass/config`
* `/etc/rofi-pass.conf`

rofi-pass only loads the first existing file.
In case no config file exists, rofi-pass uses its internal default values.
You can set the environment variable like this:

```
ROFI_PASS_CONFIG="$HOME/path/to/config" rofi-pass
```

For an example configuration please take a look at the included `config.example` file.

## Sharing passwords

Rofi-pass allows you to easily share common used passwords across multiple entries.
For example, if you have an academic account which includes several services (such as a library, Salary, Student portal etc),  all with different URL's, login forms etc. you can share one password across all of them. This is very handy when the passwords change annually.
To use this function you need to add the following line instead of the password, referencing a pass file which holds the password:

```
#FILE=PATH/to/filename
```

where PATH is relative to your password-store.

*And yes, you should slap your service provider for forcing you to share passwords across services.*

## User filename as user

If your password file has no user field you can ask rofi-pass to use the filename instead.
For example with this password file path : `web/fsf.org/rms` rofi-pass will user `rms` as your username.
To get this, you need to set `default_user` to `:filename` in your configuration.

## FAQ

### rofi pass prints garbage instead of my actual passes

Make sure to run `setxkbmap <language> <variant>` at the start of your Xorg
session.

## Alternative

jreinert has written the roughly compatible tool
[autopass](https://github.com/jreinert/autopass). It has less features, but
definately saner code.
Also he provided a nice little script called `passed` to change your
fieldnames. [link](https://github.com/jreinert/passed)
