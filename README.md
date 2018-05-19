ezldap
---------------------------------------------
[![Build Status](https://travis-ci.org/jstaf/ezldap.svg?branch=master)](https://travis-ci.org/jstaf/ezldap)
[![Coverage Status](https://coveralls.io/repos/github/jstaf/ezldap/badge.svg?branch=master)](https://coveralls.io/github/jstaf/ezldap?branch=master)

An object-oriented Python LDAP API and set of bundled scripts to make
common LDAP tasks easier.

Although several very comprehensive LDAP API's currently exist (notably
`python-ldap` and `ldap3`), neither are particularly easy to use and
generally focus more on the specifics of communicating via LDAP rather than
exposing an easy to use high-level API. This package is designed to do that.
The target audience of this package is system administrators and support
staff who are on a timeline and just need to add a user or write a quick
script in a portable manner.

So how is `ezldap` different? It is a wrapper around the `ldap3`
API that gives access to easily perform a number of high-level tasks
like searching an LDAP directory, adding a user and sending them an
email, or resetting someone's password securely. This process is
streamlined by a configuration script that autodetects as many LDAP
configuration values as possible and automates future connections to the
directory server. Additionally, additions to an LDAP directory are performed
using a set of configurable LDIF templates, making it very quick to
customize the behavior of certain actions (like adding a group). Finally,
for most tasks, a set of pre-made scripts are provided, for users who just
want to get started doing stuff.

**Example:**

```python
import ezldap

# ezldap.auto_bind() uses connection details from the "ldap-config" script
with ezldap.auto_bind() as con:
    con.add_user('username', 'groupname', 'password')
```

## installation

You'll need a copy of Python 3.4+.
No other dependencies are necessary,
though `pandas` and the OpenLDAP clients are useful for some use cases.

### centos 7:

```bash
sudo yum install epel-release
sudo yum install python34 python34-pip python34-devel
pip3 install --user git+https://github.com/jstaf/ezldap.git
```

### ubuntu 16.04:

```bash
sudo apt update
sudo apt install python3 python3-pip python3-dev
pip3 install --user git+https://github.com/jstaf/ezldap.git
```

Configure your LDAP connection details with `ldap-config` before using the package.
You do not need to run this script as root.
The connection details/LDIF templates/etc. for `ezldap` are stored in `~/.ezldap`.
Though you can store your bind password here for convenience,
I do not recommend doing so, as it will be stored in cleartext.
You may wish to configure the LDIF templates under `~/.ezldap`
as well if you intend to use this package's add object functionality.

## using this package

Refer to the scripts' command-line help (`./script-name -h`) for usage info and documentation.
Keep in mind this package is under active development and may not be appropriate for all use cases.
Functionality is constantly being added, and the API should not yet be considered stable.

For further documentation check out the [wiki](https://github.com/jstaf/ezldap/wiki)

## running tests

This package runs a lot of tests against a slapd docker image using `pytest`.
You'll need [Docker](https://www.docker.com/community-edition),
[Docker Compose](https://docs.docker.com/compose/install/),
and and the `openldap-clients` packages as well
(see `.travis.yml` for a full list of dependencies).

Once all of that is installed, you can run the tests using `pytest`.
