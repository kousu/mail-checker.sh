# mail-checker.sh

Verifies your mail server setup.

## Installation

### From source

You need:

- a linux-based system (sorry! I like BSD too; I'll get to it eventually)
- `swaks`
- `curl`
- ``


## Usage

E-mail is a fairly flexible and therefore arcane and difficult suite of protocols. Especially, handling e-mail spam has demanded developing strong repuation systems, and vetting these is hard.

Some terminology:

* MX: a Mail eXchanger; i.e. an smtp server, running on port 25.
* `hostname`: the name of the MX; expected to be in DNS, and optionally written on a TLS certificate.
* `domain`: the name of the domain hosting on the MX.
    * for simple setups, the domain name == hostname, so the "A" DNS record for `domain` is the same as the "MX" record, so if you have e.g. hostname=forum.example.org, then mail will appear to come from user@forum.example.org
    * for more complex setups, the mail server need not be the same as the server with the primary domain attached, so mail will appear to come from user@example.org.
* `ip`: the IP address of the server to actually connect to to test
* TLS: encryption.


`mail-checker` can check the site .. 

```
mail-checker
```

Test the local server which should be its own MX.

Gets `hostname` and `ip` directly, and assumes `domain` = `hostname` is the *same* as the hostname, which should be the case for all simple servers.

This is the recommended way to use `mail-checker`.

```
mail-checker hostname
```

Test a remote server that is its own MX.

Assumes `domain` = `hostname`. Gets the `ip` from the *DNS* for `hostname`.

```
mail-checker hostname domain [ip]
```

Test a server with a separate MX. get the IP address from DNS 

Optionally, for full control, the `ip` of the MX can be specified directly, instead of relying on finding it in DNS. This lets you to test your DNS entries, work around unstable DNS results, or target a specific server if you have multiple backup MXes.


The recommended way to use this:

* Install `mail-checker` on your MX and run `mail-checker` there
* Correct any problems identified
* Install `mail-checker` on a different server and run `mail-checker example.org`


### Incoming vs Outgoing

The tests included here test both incoming mail (mostly: can we connect to your server?) and outgoing mail (mostly: does the repuation of this server look alright?). **Not all mail servers need to be both** and you can ignore the errors for one if you're not using it.

Servers *should* accept incoming mail to at least postmaster@`domain` and/or abuse@`domain` to receive complaints before they turn into bans, but it's not mandatory. For deploying many apps, like WordPress or NextCloud, output via email is key, but no input is returned via email.

