# Firefly-over-LSDB

Firefly-over-LSDB (working title) is a web service being developed at [IPAC-Caltech](http://ipac.caltech.edu/)
that comprises a [Firefly](https://github.com/Caltech-IPAC/firefly) UI frontend and an
[lsdb-server](https://github.com/IPAC-SW/lsdb-server/) [^1] (working title) backend to query
[HATS](https://hats.readthedocs.io/) catalogs.
lsdb-server uses the python libraries [hats](https://hats.readthedocs.io/) and [lsdb](https://docs.lsdb.io/)
to work with HATS catalogs.
Firefly provides a GUI for browsing the available catalogs and their schemas, submitting queries,
and exploring the results.

## Quickstart

To deploy the web service, ensure that you have [Docker](https://docs.docker.com/) and
[Docker Compose](https://docs.docker.com/compose/) installed.
Then, from a terminal, navigate to this directory and execute the following command to pull the images from
DockerHub (one each for Firefly and lsdb-server) and run them in containers:

```sh
$ docker compose up
```

Once the containers are running, visit the service at <http://localhost/local/> [^2] and click on the
tab called "LSDB Query" [^3].

To bring the service down, go to the terminal and press *Ctrl-C* to stop the containers.
You can also delete the containers and images if you want:

```sh
$ # Use 'Ctrl-C' to stop the containers. Then:
$
$ # Delete the containers (optional)
$ docker compose down
$
$ # Delete the images (optional)
$ docker rmi ipac/firefly troyraen/lsdb-server
```

## Notes

[^1] lsdb-server:

- The [lsdb-server](https://github.com/IPAC-SW/lsdb-server/) code repo is temporarily private.
  Public release is pending approval from Caltech.
- As of 7 Feb 2025, there are three known bugs with open issues in the repo:
    1. If data is returned that contains nulls in an integer column, a type conversion will fail and
       throw an error.
    2. Submitting two constraints in the "constraints" box next to a given column
       (e.g., entering `> 10 and < 15`) will throw an error. As a workaround, submit the constraints
       separately in the "additional constraints" box at the bottom (e.g., `w1mpro>10 and w1mpro<15`
       for a column called 'w1mpro').
    3. If a constraint is submitted for a given column, that column must also be selected for return
       (i.e., the box to the left of the column name must be checked), otherwise it will throw an error.
- If you find additional bugs:
    - If the repo has been made public, please open an issue.
    - Otherwise, please send an email to raen@ipac.caltech.edu.

[^2] Service url:

- The domain is likely to be 'localhost' when run and accessed from a personal computer.
  It is also possible to expose the service for external access, and in that case the domain will
  depend on your setup.
- The path '/local/' maps to the `firefly-html/` directory included here.
  This is here for the sole purpose of changing the tab name to "LSDB Query" for clarity and is a
  temporary solution while the Firefly team implements a better way to configure it.
  The standard Firefly page can be seen at the path '/firefly/' -- the tab called "IRSA Catalogs"
  on that page is exactly the same as "LSDB Query", except for the name.
  An environment variable set in `compose.yaml` overrides Firefly's standard configuration of that tab to
  point it at lsdb-server.

[^3] Tabs:

- The tabs seen at '/local/' can be configured by altering the `firefly-html/index.html` file.
    - The label "LSDB Query" can be changed to whatever you'd like.
    - Other tabs (e.g., "Images") with standard Firefly functionality can be enabled by uncommenting
      the relevant line(s).
- If tabs are enabled but you don't see them in the UI, click on the hamburger menu icon (top left,
  three parallel lines) to access them.
