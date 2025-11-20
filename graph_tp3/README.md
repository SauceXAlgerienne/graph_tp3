I had a problem I solved.

The container was running, but GDS was not actually being installed, because PowerShell was mangling the NEO4J_PLUGINS value.

On Windows PowerShell, we must escape the quotes like this:


1. Stop & remove the current container

In PowerShell:
docker stop neo4j-twitch
docker rm neo4j-twitch


2. Start a fresh container WITH GDS (correct PowerShell syntax)

Still in PowerShell, run this exactly as one single line:
docker run --name neo4j-twitch -p 7474:7474 -p 7687:7687 -e NEO4J_AUTH=neo4j/password -e 'NEO4J_PLUGINS=[\"apoc\",\"graph-data-science\"]' -e NEO4J_ACCEPT_LICENSE_AGREEMENT=yes -d neo4j:5.20


3. Check that GDS is being installed

Run: docker logs neo4j-twitch



Scroll a bit; you should see lines like:

Installing Plugin 'graph-data-science' ...

Installing Plugin 'apoc' ...

Once that’s done and the DB has finished starting (you’ll see “Remote interface available at …” or similar), go to:

http://localhost:7474

Log in (neo4j / password), and in the Browser run: RETURN gds.version();

We’ll get a version string (e.g. 2.x.x) and we’re good, we can continue the exercise in your notebook.