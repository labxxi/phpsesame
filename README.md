=========
phpsesame
=========

Fork of the alex latchford sesame interface https://github.com/alexlatchford/phpSesame.
Inspired by the work of Julian Klotz https://github.com/julianklotz/phpSesame.git,
and Andreas Thalhammer https://github.com/athalhammer/phpSesame.git

Requirements
============

- `PHP 5+ <http://php.net/>`_ - (There shouldn't be any subversion dependencies, but I haven't checked thoroughly)
- `HTTP_Request2 <http://pear.php.net/package/HTTP_Request2>`_
- `semsol/ARC2 <https://github.com/semsol/arc2>` library, available here on github, is strongly recommended to parse results or generate rdf.



Examples
========

I am assuming at this point you have installed and configured Sesame, have a repository set up and the REST API functioning correctly. If not then please consult the `Sesame documentation <http://www.openrdf.org/doc/sesame2/users/>`_.

Using the Library
-----------------

To get the library up and running all you need is::

	require_once "path/to/phpSesame/phpSesame.php";

	$sesame = array('url' => 'http://localhost:8080/openrdf-sesame', 'repository' => 'exampleRepo', 'charset' => 'UTF-8');
	$store = new phpSesame($sesame['url'], $sesame['repository'],  $sesame['charset']);

You can change the repository you are working on at any time by calling::

	$store->setRepository("newRepo");

Charset param is used for both content type and accept headers params. You can set them specifically by calling::

        $store->setAcceptCharset();
        $store->setContentCharset();
        

Querying a Store
----------------

The simplest way to query a store is::

	$sparql = "PREFIX foaf:<http://xmlns.com/foaf/0.1/>
	SELECT ?s ?o WHERE { ?s foaf:name ?o } LIMIT 100";
	$resultFormat = phpSesame::SPARQL_XML; // The expected return type, will return a phpSesame_SparqlRes object (Optional)
	$lang = "sparql"; // Can also choose SeRQL (Optional)
	$infer = true; // Can also choose to explicitly disallow inference. (Optional)

	$result = $store->query($sparql, $resultFormat, $lang, $infer);
        

Using ARC2 for results::   
     
        $parser = ARC2::getSPARQLXMLResultParser();
        $parser->parse('', $result);
        foreach($parser->getRows() as $row) {
                echo "Subject: " . $row['s'] . ", Object: " . $row['o'] . ".";
        }
	

Documentation will be updated with new features examples soon.
