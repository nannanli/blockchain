---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Developing applications
{: #dev_app}
With applications, you can invoke the chaincode to query or update a channel-specific ledger in your blockchain network.
{:shortdesc}

## Setting up application development environment
You need to install software prerequisites to develop applications that can interact with the blockchain network on {{site.data.keyword.cloud}}.
{:shortdesc}

*	Git ([Git download page ![External link icon](images/external_link.svg "External link icon")](https://git-scm.com/downloads){:new_window})  
	Git is a version control tool to familiarize yourself with, both for chaincode development and software development in general. Also, git bash, which is installed with git on Windows, is an excellent alternative to the the Windows command prompt.

*	GoLang ([GoLang download page ![External link icon](images/external_link.svg "External link icon")](https://golang.org/dl){:new_window})  
	The GoLang installation installs a set of Go CLI tools, which are very useful to write chaincode. For example, the `go build` command allows you to check that your chaincode actually compiles before you attempt to deploy it to a network.

	Follow the [Installation instructions ![External link icon](images/external_link.svg "External link icon")](https://golang.org/doc/install){:new_window} to properly set the environment variables. Check your `GOPATH` using the below command. Note that your `GOPATH` does not need to match the example, it only matters that you have this variable set to a valid directory on your file system.

	```
	$ echo $GOPATH
	C:\gopath
	```
	{:codeblock}

	You can then verify your GoLang installation by building GoLang code with the [hello world ![External link icon](images/external_link.svg "External link icon")](https://golang.org/doc/install#testing){:new_window} example.

* Java ([Java download page ![External link icon](images/external_link.svg "External link icon")](https://java.com/en/download/){:new_window}).
*	Node.js ([Node.js download page ![External link icon](images/external_link.svg "External link icon")](https://nodejs.org/en/download/){:new_window}).

Chaincode development is currently supported in Node.js, Go, and Java.

## Generating the client-side certificates
We won't delve into the minutiae of x509 and public key infrastructure; there are plenty of external resources for that. Suffice it to say that communication flows in Fabric use sign/verify operations at every touchpoint. As such, any client that sends calls (that is, ledger queries or updates) to the network will need to sign payloads and attach a properly signed x509 certificate for verification purposes. The private key and signed certificate, along with an MSP identifier and the Certificate Authority (CA) root certificate, make up what is referred to as the "user context" object.

What's important here is the process of "enrollment", in which the keys and certs that allow for the object to be formed are retrieved from the appropriate CA. After you form the user context object, you can call an API from your application to "set" or "get" this user context. At this point, the application (i.e. client) is equipped with all the necessary artifacts and is ready to communicate with the network. We'll look at two approaches for retrieving the keys and certs: commande line and SDK.

### Command line
Using command line is the simpler of the two approaches.

1. Follow the instructions to build the [Fabric CA client ![External link icon](images/external_link.svg "External link icon")](http://hyperledger-fabric-ca.readthedocs.io/en/latest/users-guide.html). This step allows you to communicate with a CA Server and receive properly-formatted certificates and keys.

2. Download the TLS certs from {{site.data.keyword.cloud_notm}} depending on the service plan, location, and cluster that you use.
  - Root TLS Cert for Starter Plan  
    - US: [us01.blockchain.ibm.com.cert ![External link icon](images/external_link.svg "External link icon")](http://blockchain-certs.mybluemix.net/us01.blockchain.ibm.com.cert "us01.blockchain.ibm.com.cert"); [us02.blockchain.ibm.com.cert ![External link icon](images/external_link.svg "External link icon")](http://blockchain-certs.mybluemix.net/us02.blockchain.ibm.com.cert "us02.blockchain.ibm.com.cert")
    - UK: [uk01.blockchain.ibm.com.cert ![External link icon](images/external_link.svg "External link icon")](http://blockchain-certs.mybluemix.net/uk01.blockchain.ibm.com.cert "uk01.blockchain.ibm.com.cert"); [uk02.blockchain.ibm.com.cert ![External link icon](images/external_link.svg "External link icon")](http://blockchain-certs.mybluemix.net/uk02.blockchain.ibm.com.cert "uk02.blockchain.ibm.com.cert")
    - Sydney: [aus01.blockchain.ibm.com.cert ![External link icon](images/external_link.svg "External link icon")](http://blockchain-certs.mybluemix.net/aus01.blockchain.ibm.com.cert "aus01.blockchain.ibm.com.cert"); [aus02.blockchain.ibm.com.cert ![External link icon](images/external_link.svg "External link icon")](http://blockchain-certs.mybluemix.net/aus02.blockchain.ibm.com.cert "aus02.blockchain.ibm.com.cert")
  - [Root TLS Cert for Enterprise Plan ![External link icon](images/external_link.svg "External link icon")](https://blockchain-certs.mybluemix.net/3.secure.blockchain.ibm.com.rootcert)

  Save the contents to a folder, for example ``$HOME/tls``. This step allows the data flowing to be encrypted on the wire.

3. Open the **Connection Profile** JSON file from your **Overview** screen in the Network Monitor, and find the following variables:
  * URL for CA: ``url`` under `certificateAuthorities`
  * Admin user ID: ``enrollId``
  * Admin password: ``enrollSecret``
  * CA Name: ``caName``

4. Using the Fabric CA client, we can send an "enroll" call to our Certificate Authority by passing in the TLS certs path and the four strings above with the following command:
  ```
  $GOPATH/bin/fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <ca_name> --tls.certfiles <tls_cert_path>
  ```
  {:codeblock}

  The ``fabric-ca-client`` binary is placed in ``$GOPATH/bin``, so you will need to be in the proper location on your local machine when you run this command.  A real call might look similar to the following example command:
  ```
  ./fabric-ca-client enroll -u https://admin:B84F2C5436@tor-zbc01a.3.secure.blockchain.ibm.com:23042 --tls.certfiles /Users/XYZ/Downloads/3.secure.blockchain.ibm.com.rootcert --caname PeerOrg1CA
  ```
  {:codeblock}

5. Find your admin certificate in `$HOME/.fabric-ca-client/msp/signcerts/cert.pem`. You can then upload the admin certificate to your blockchain network from the Network Monitor. For more information about adding certificates, see [the "Certificates" tab of "Member" screen](v10_dashboard.html#members) in the Network Monitor.

  You can also find CA root certificate and admin private key in the following directories:
  * CA root certificate: `$HOME/.fabric-ca-client/msp/cacerts/--<ca_name>.pem`
  * The admin private key: `$HOME/.fabric-ca-client/msp/keystore/<>_sk file`

### SDK
There are a few Fabric repositories that contain excellent resources and scripts for understanding how to programmatically interact with a Certificate Authority. The ``fabric-samples`` repo contains the `balance transfer` example and the ``fabric-sdk-node`` repo has a series of CA Services tests. If you intend to issue your enrollment requests on the application side, then you need to fully understand the APIs that need to be exposed within the ``fabric-ca-client`` and ``fabric-client`` packages. Use these scripts and repos as a baseline for structuring your app.

Let's take a quick look at a few key snippets from the `balance transfer` example:

First, we need to create our client object and set a key-value store instance where our certs and keys will be parked. We can do this with a simple factory method -- ``newCryptoSuite`` - that extends to the ``Client`` class from ``BaseClient``. Here's a quick look at the code:

```
# <PUBLIC_PRIVATE_KEY_PATH> denotes the path on your local machine where you wish to store your key and cert
let cryptoSuite = hfc.newCryptoSuite();
cryptoSuite.setCryptoKeyStore(hfc.newCryptoKeyStore({path: <PUBLIC_PRIVATE_KEY_PATH>)}));
client.setCryptoSuite(cryptoSuite);
```
{:codeblock}

The common practice would be to export an environment variable that defines the key/value path on your machine and pass it to the above function. Now that we've defined our KVS, let's use a few methods from the ``FabricCAServices`` class. This class is an implementation of the Fabric CA client, and as such it will allow us to communicate with the CA Server. First, we need to pass some information to our CA client, namely the CA URL:

```
# the caURL can be defined manually or by setting an environment variable
# the copService variable is defined at the top of the program
let caUrl = "https://XXX:7054";
var caClient = new copService(caUrl, null, '' /* default CA */, cryptoSuite);
```
{:codeblock}

Next, we will actually send the "enroll" call to the CA Server. This will return to the client a private key and a public key that has been wrapped in an x509 cert and signed by the targeted CA -- we call this the signcert or enrollment certificate. This enrollment cert or "eCert" is the key piece, because it allows network entities to verify transactions and calls originating from the client:

```
return caClient.enroll({
enrollmentID: username,
enrollmentSecret: password
```
{:codeblock}

And the final task is actually setting the crypto suite and building the user context:

```
	member.setCryptoSuite(client.getCryptoSuite());
	return member.setEnrollment(enrollment.key, enrollment.certificate, getMspID(userOrg));
}).then(() => {
	return client.setUserContext(member);
```
{:codeblock}

You can then upload the admin certificate to your blockchain network from the Network Monitor. For more information about adding certificates, see [the "Certificates" tab of "Member" screen](v10_dashboard.html#members) in the Network Monitor.

## Developing applications
{: #developing-applications}

An easy way to start to develop applications is to use sample chaincode and applications as a template for creating your own business solution. You can find sample applications for the blockchain networks on {{site.data.keyword.cloud_notm}} in [Sample applications](howto/prebuilt_samples.html). You can also start to develop applications from scratch based on your own business needs.

You can develop your application in JavaScript or Java, and leverage the available APIs in the Hyperledger Fabric Client SDKs to enable interaction between your application and your network.  An application needs to include at least the following information:
* Name and version of the chaincode to invoke.
* API endpoint information of your network resources, including orderers, CAs, and peers.
* Functions to query or update the ledger in the network.  If you want high availability, you need to consider node failover in your application.

### Using Fabric SDKs
{: #use-sdks}

Fabric offers Node.js SDK and Java SDK currently and will support more programming languages, such as Python, REST, or GO, in future releases. For more information about Fabric SDKs and how to use them, see the following documentation:

- [Hyperledger Fabric Node SDK documentation ![External link icon](images/external_link.svg "External link icon")](https://fabric-sdk-node.github.io/){:new_window}
- [Hyperledger Fabric Java SDK documentation ![External link icon](images/external_link.svg "External link icon")](https://github.com/hyperledger/fabric-sdk-java){:new_window}

### Setting timeout values in Fabric SDKs
{: #set-timeout-in-sdk}

Fabric SDKs set default timeout values in client applications for events in the blockchain network. See the following example about default timeout settings in Fabric Java SDK. The file path is `src\main\java\org\hyperledger\fabric\sdk\helper\Config.java`.

```
    /**
     * Timeout settings
     **/
    public static final String PROPOSAL_WAIT_TIME = "org.hyperledger.fabric.sdk.proposal.wait.time";
    public static final String CHANNEL_CONFIG_WAIT_TIME = "org.hyperledger.fabric.sdk.channelconfig.wait_time";
    public static final String TRANSACTION_CLEANUP_UP_TIMEOUT_WAIT_TIME = "org.hyperledger.fabric.sdk.client.transaction_cleanup_up_timeout_wait_time";
    public static final String ORDERER_RETRY_WAIT_TIME = "org.hyperledger.fabric.sdk.orderer_retry.wait_time";
    public static final String ORDERER_WAIT_TIME = "org.hyperledger.fabric.sdk.orderer.ordererWaitTimeMilliSecs";
    public static final String PEER_EVENT_REGISTRATION_WAIT_TIME = "org.hyperledger.fabric.sdk.peer.eventRegistration.wait_time";
    public static final String PEER_EVENT_RETRY_WAIT_TIME = "org.hyperledger.fabric.sdk.peer.retry_wait_time";
    public static final String EVENTHUB_CONNECTION_WAIT_TIME = "org.hyperledger.fabric.sdk.eventhub_connection.wait_time";
    public static final String EVENTHUB_RECONNECTION_WARNING_RATE = "org.hyperledger.fabric.sdk.eventhub.reconnection_warning_rate";
    public static final String PEER_EVENT_RECONNECTION_WARNING_RATE = "org.hyperledger.fabric.sdk.peer.reconnection_warning_rate";
    public static final String GENESISBLOCK_WAIT_TIME = "org.hyperledger.fabric.sdk.channel.genesisblock_wait_time";

    ...

    // Default values
    /**
     * Timeout settings
     **/
    defaultProperty(PROPOSAL_WAIT_TIME, "20000");
    defaultProperty(CHANNEL_CONFIG_WAIT_TIME, "15000");
    defaultProperty(ORDERER_RETRY_WAIT_TIME, "200");
    defaultProperty(ORDERER_WAIT_TIME, "10000");
    defaultProperty(PEER_EVENT_REGISTRATION_WAIT_TIME, "5000");
    defaultProperty(PEER_EVENT_RETRY_WAIT_TIME, "500");
    defaultProperty(EVENTHUB_CONNECTION_WAIT_TIME, "5000");
    defaultProperty(GENESISBLOCK_WAIT_TIME, "5000");
    /**
     * This will NOT complete any transaction futures time out and must be kept WELL above any expected future timeout
     * for transactions sent to the Orderer. For internal cleanup only.
     */
    defaultProperty(TRANSACTION_CLEANUP_UP_TIMEOUT_WAIT_TIME, "600000"); //10 min.
```
{:codeblock}

However, you might need to change the default timeout values in your own application. For example, when your application invokes a transaction that needs more than 5000 ms, which is the default timeout value for event hub connection, to response, you will get a failing error because the invoke event ends at 5000 ms before the transaction completes. You can set the system property to overwrite the default values from your client application. Because the default values are initialized before you set the system property, the system property might not take effect. Therefore, you need to set the system property for timeout in a static construct in your client application. See the following example on changing timeout value for event hub connection to 15000 ms in Fabric Java SDK. The file path is `src\main\java\org\hyperledger\fabric\sdk\helper\Config.java`.

```
 public static final String EVENTHUB_CONNECTION_WAIT_TIME = "org.hyperledger.fabric.sdk.eventhub_connection.wait_time";
 private static final long EVENTHUB_CONNECTION_WAIT_TIME_VALUE = 15000;

 static {
     System.setProperty(EVENTHUB_CONNECTION_WAIT_TIME, EVENTHUB_CONNECTION_WAIT_TIME_VALUE);
 }
```
{:codeblock}


## Adding network API endpoints to your application
You need to add API endpoints of your network resources to your application so that it can interact with your {{site.data.keyword.blockchain}} network on {{site.data.keyword.Bluemix_short}}.  If you don't have a {{site.data.keyword.blockchain}} network on {{site.data.keyword.Bluemix_short}}, you can create one with either Starter Plan or Enterprise Plan. For more information, see [Govern Starter Plan network](get_start_starter_plan.html) and [Govern Enterprise Plan network](get_start.html).

You can find the API endpoints in the connection profile of your network. The connection profile is in JSON format and contain the API endpoint information and enrollIDs/secrets for your network resources, including orderer, CA, and peer nodes. Your application will interact with peers and other network resources through these API endpoints.

1. Retrieve the API endpoint information of your network resources from your Network Monitor with one of the following methods:
	* To get the API endpoint information that is specific to a chaincode, in the specific channel screen, on which the chaincode is running, locate the chaincode and click the **JSON** button.
	![API endpoints per chaincode](images/channel_chaincode.png "API endpoints per chaincode")
	* To get a complete set of API endpoint information about all your network resources, click the **Connection Profile** button in the "Overview" screen.
	![Connection Profile in Network Monitor](images/service_credentials.png "Connection Profile in Network Monitor")

2. Locate the API endpoint information of your network resources, which is similar to the following example:
	```
	"peers": {
            "fabric-peer-org3-18400c": {
                "url": "grpcs://tor-zbc06c.3.secure.blockchain.ibm.com:18400",
                "eventUrl": "grpcs://tor-zbc06c.3.secure.blockchain.ibm.com:13547",
                ...
	```
	{:codeblock}
	**Note**: If you want to target additional peers in the network, for example, you require endorsement from a peer that does not belong to your organization, then you need to obtain the correct API endpoint information of those peers.  You also need to store the CA cert for the other organization to verify responses that return to your application. This information is not exposed in your connection profile, therefore you must contact the appropriate admin for the CLoud Foundry Org and acquire this information in an out-of-band operation. The ordering service URL is common across the network; you don't need any member-specific information for ordering service.

3. Plug the API endpoint information into a configuration file of your application as shown in the following example:
	```
	orderer_url: 'grpcs://fft-zbc01a.4.secure.blockchain.ibm.com:18603'
	```
	{:codeblock}

## Using CouchDB indices

If your network uses CouchDB and you want to take advantage of the CouchDB indexing feature (which improves the performance of CouchDB) the indexes need to be packaged with the chaincode.

To learn more about CouchDB and how to set up indices, consult the [Fabric documentation on CouchDB ![External link icon](images/external_link.svg "External link icon")](http://hyperledger-fabric.readthedocs.io/en/release-1.1/couchdb_as_state_database.html)

## Hosting applications
You can host your application on your local file system or push it to {{site.data.keyword.Bluemix_notm}}. To push your application to {{site.data.keyword.Bluemix_notm}}, complete the following steps:
1. Install the [Cloud Foundry Command Line Installer ![External link icon](images/external_link.svg "External link icon")](https://github.com/cloudfoundry/cli/releases).  Test your installation with the `cf` command.
    * If your installation is successful, you should see a bunch of text output in your terminal.
    * If you see "command not found", your installation was either not successful or CF is not added to your system path.
2. Set up API endpoint and log in with your {{site.data.keyword.Bluemix_notm}} ID and password by issuing the following commands:
    ```
    > cf api https://api.ng.bluemix.net
    > cf login
    ```
    {:codeblock}
3. Browse to the directory of your application, and push your application by issuing the following command. This can take several minutes depending on your application size. You will see logs from {{site.data.keyword.Bluemix_notm}} in your terminal; they will cease once the application has successfully launched.
	```
	> cf push YOUR_APP_NAME_HERE
	```
	{:codeblock}
	You can check your application logs by issuing one of the following commands:
	* `> cf logs YOUR_APP_NAME_HERE`
	* `> cf logs YOUR_APP_NAME_HERE --recent`


## Disconnecting your application from the network
{: #disconnect}
Complete the following steps to remove the connection between your application and the {{site.data.keyword.blockchain}} network on {{site.data.keyword.Bluemix_short}}.
1. Remove the API endpoint information from your application configuration file. For reference, see [Adding network API endpoints to your application](#adding-network-api-endpoints-to-your-application).
2. Delete your chaincode container.
	1. In the "Channel" screen of the Network Monitor, locate the channel where your chaincode is installed.
	2. In the specific channel screen, locate the chaincode you want to disable.
	3. Click the **Delete** button, and click **Submit** in the chaincode deletion panel. Your chaincode container will be removed.
	![Delete a chaincode](images/channel_chaincode.png "Delete a chaincode")
