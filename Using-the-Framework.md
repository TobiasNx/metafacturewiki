This page explains how to create a Metafacture objects and how to assemble them to form a processing pipeline. We use as an example a simple pipeline containing a Metamorph instance.

# Creation

A new Metamorph is created by calling `build()` on MetamorphBuilder.

`final Metamorph metamorph = new Metamorph("definition.xml");`

Please note that the Metamorph object is not thread-safe.



# Wiring
The Metamorph object acts as a pipe element in the data stream. 
 This means that we must wire it to a data source  and a data sink. 
`// create necessary objects
final PicaReader reader = new PicaReader();
final Metamorph metamorph = new Metamorph("defnition.xml");
final ListMapWriter writer = new ListMapWriter();

//wire them
reader.setReceiver(metamorph).setReceiver(writer);

//start processing
reader.read(input);
`

First all elements of the processing chain are created.
Pipeline elements are connected calling . The call `setReceiver()` returns
its argument, preserving the respective type. Thus the calls can be chained to
build up a pipeline as shown in the listing. Finally the processing is started
by calling the respective method on the data source/reader. The method name
depends on the reader. In the Metamorph project `read()` is used by
convention.



The following code snippet shows a few more sophisticated wiring patterns, such
as adding an additional element, junctions or splitters.

`
//adding logging
reader.setReceiver(new LogPipe()).setReceiver(metamorph).setReceiver(writer);

//adding a tee junction
reader.setReceiver(new Tee()).setReceivers(writer1, writer2);

//splitting based on a metamorph description
final Splitter splitter = new Splitter("morph/typeSplitter.xml");
reader.setReceiver(splitter).setReceiver("Tn", writer1);
splitter.setReceiver("Tp", writer2);
`

# Error Handling
If an exception occurs during the processing of a stream of records, it is back
propagated to the first element in the chain. This normally means that
processing is terminated which may not be the preferred action. Imagine
processing a million records. One normally prefers to log any error but continue
the processing.
For this reason an error handler may be registered with the Metamorph object. It
catches all exceptions occurring in the Metamorph object and below. 

`
metamorph.setErrorHandler(new MetamorphErrorHandler() {
	@Override
	public void error(final Exception e) {
		// TODO fill in your error handling code
	}
});
`