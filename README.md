# jupancon

Database Connectors and SQL magics for Jupyter lab. `jupancon` = Jupyter + Pandas + Connectors.

(What follows is still WIP)

### Features

- Connector to Redshift
- Connector to Bigquery
- Optional automatic tunnel setting through an SSH Bastion
- Querying capabilities
- Jupyter Magics for querying

### Install

```
pip install jupancon
```
### Configure

Write a `~/.jupancon/config.yml` YAML file that looks similar to the following C&P from my actual config file (heavily censored for obvious reasons):

```
default: my-redshift-cluster

my-redshift-cluster: 
    type: redshift
    host: XXXXXX.XXXXXX.XXXXXXX.redshift.amazonaws.com
    # explicitly setting redshift port (optional)
    port: 5439
    user: XXXXXXXX
    pass: XXXXXXXX
    dbname: XXXXXX


my-gcp:
    type: bigquery
    project: XXXXX-XXXXX-123456


my-redshift-behind-sshbastion:
    type: redshift
    use_bastion: true
    bastion_server: censored.bastion.server.com
    bastion_user: XXXXXX
    bastion_host: XXXXXX.XXXXXX.XXXXXX.redshift.amazonaws.com
    host: censored.main.server.com
    user: XXXXXXXX
    pass: XXXXXXXX
    dbname: XXXXXX



```

# How to use

### Regular usage

```
from jupancon import query, list_schemas, list_tables

list_schemas()

list_tables()

query("select * from foo")
```

### Magical usage

```
from jupancon import load_magics

load_magics()
```

```
select * from foo
```

```
df = %select * from foo
```

```
%%sql

select * 
from foo
where cond = 1
and whatever
```

# Development

### TODO list

- Complete docs (low level stuff, exhaustive features, maybe sphinx/rdd?)
- Add animated gifs to docs 
- Add query monitoring and cancelling functionality+

### Features that aren't worth adding right now

- Autocomplete and autodiscover of databases is possible, but not trivial at all. Not worth it until the TODO list above is tackled. See https://github.com/jupyter-lsp/jupyterlab-lsp for a successful example.
- Because of the current architecture of Jupyter Lab, syntax highlighting is not feasible to add (as it was in Jupyter Notebook). This might change in the future. See https://github.com/jupyterlab/jupyterlab/issues/3869


### A note about Unit Testing

I'm a big fan of unit testing, but this library is hard to test because all the databases I query are either tests that cost me money or private (clients) databases. Any ideas on how to write an **open source** set of unit tests (especially for Redshift and BigQuery) are very welcome.
