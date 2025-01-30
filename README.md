# scsh

`scsh` is a helper utility to initiate VNC connections from the command line on MacOS, using a similar configuration file to `ssh`.

The original goal was to be able to use the same `Host` alias to connect to my home server regardless of if I'm local or remote. This tool aims to bring that functionality to VNC as well.

Simply set up a configuration file (similar to SSH, but a limited feature set), and run the command `scsh <host>`!

I'm sure there were other, better ways I could've set this up, but the entire point of it is that I wanted to be able to use my actual `ssh` configuration to initiate VNC connections, instead of having to poke through the Screen Sharing GUI with my cursor.

## Installation

Copy the `scsh` binary to a location in your `$PATH`.

Optionally, create a `$HOME/.scsh/` directory with a `config` file inside it.

## Usage

### Command Line

```bash
# scsh [-vp] <host>
#   -v: Verbose
#   -p: Print output (do not connect).

# returns foo@bar:baz
scsh -p foo@bar:baz

# returns 'foo@bar'
scsh -p foo@bar:5900

```

### Configuration File

Lives at `$HOME/.scsh/config`.

See `.scsh/config_sample_commented)` in this repo for an example in context!

Much like the `ssh` config, this config file is parsed in order from top to bottom and the tool accepts the first value it finds as the final value (that is, it doesn't continue parsing and overwrite previous values). Matching is case-sensitive because that's how `ssh` does it. Most `ssh` configuration options probably won't work, but the somewhat strange options I use in my setup for the `ssh` config does, at least.

> A Match block will attempt to match "originalhost" (i.e. whatever string you provided after 'scsh' on the command line) against any term in the following comma-separated list. If it finds a match, it runs the command following the exec call. If that command succeeds (exits 0), the configuration settings in the Match block are used to initiate the VNC connection.

```bash
Match originalhost '[MATCH TERM 1],[MATCH TERM 2],...' exec "<command exiting 0 or 1>"
    User username                # account username
    HostName hostname_when_local #(ex. 'MyComputer.local', without quotes)
    Port *optional*              # (optional - defaults to 5900 if left blank)
```

> A Host block will attempt to match whatever string you provided after the scsh call on the command line against any of the terms in the space-separated list following "Host" (again, like ssh). If it finds a match, the configuration settings in the Host block are used to iniated the VNC connection.

```bash
Host [MATCH TERM 1] [MATCH TERM 2]...
    User username                   # account username
    HostName hostname_when_remote   #(ex. 'MyHome.duckdns.org', without quotes)
    Port *optional*                 #(optional - defaults to 5900 if left blank)
```

## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

## License

[MIT](https://choosealicense.com/licenses/mit/)
