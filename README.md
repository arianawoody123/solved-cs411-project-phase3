Download Link: https://assignmentchef.com/product/solved-cs411-project-phase3
<br>
You are required to develop a simplified version of the TextSecure protocol, which provides forward secrecy and deniability. Working in the project will provide you with insight for a practical cryptographic protocol, a variant of which is used in different applications such as WhatsApp.

<h1>1      Introduction</h1>

The project has three phases:

<ul>

 <li><strong>Phase I </strong>Developing software for the Registration and the Station-to-Station (STS) protocols. All coding development will be in Python programming language.</li>

 <li><strong>Phase II </strong>Developing software for receiving messages from other clients</li>

 <li><strong>Phase III </strong>Developing software for sending messages from other clients More information about project is given in the subsequent section.</li>

</ul>

<h1>2 Phase I: Developing software for the Registration and Stationto-Station Protocols</h1>

In this phase of the project, you are required to upload one file: “Client.py”. You will be provided with “Client basics.py”, which includes all required communication codes.

In the Registration protocol (see below) you will register your long term public key with a server. You will also simulate the STS protocol with the same server. The server is accessible via cryptlygos.pythonanywhere.com. You may find the connection details in “Client basics.py”. JSON format should be used in all communication steps.

In the Station-to-Station (STS) protocol, each party must have a long term public and private key pair to sign messages and verify signatures. A variant of Elliptic Curve Digital Signature Algorithm (ECDSA) will be used with NIST-256 curve in this project. See Section 2.3 for the description of the variant of the ECDSA algorithm that will be used in the project. You should select “secp256k1” for the elliptic curve in your python code. <strong>Note that you will NOT use the ECDSA algorithm in the slides.</strong>

<h2>2.1     Registration</h2>

The long term public key of the server <em>Q<sub>SL </sub></em>is given below.

X:0xc1bc6c9063b6985fe4b93be9b8f9d9149c353ae83c34a434ac91c85f61ddd1e9

Y:0x931bd623cf52ee6009ed3f50f6b4f92c564431306d284be7e97af8e443e69a8c

In this part, firstly you are required to generate a long-term private and public key pair <em>s<sub>L </sub></em>and <em>Q<sub>L </sub></em>for yourself. The key generation is described in “Key generation” algorithm in Section 2.3. Then, you are required to register with the server. The registration operation consists of four steps:

<ol>

 <li>After you generate your long-term key pair, you should sign your ID (e.g. 18007). The details of the signature scheme is given in the signature generation algorithm in Section 2.3. Then, you will send a message, which contains your student ID, the signature tuple and your long-term public key, to the server. The message format is</li>

</ol>

{‘ID’: stuID, ‘H’: h, ‘S’: s, ‘LKEY.X’: lkey.x, ‘LKEY.Y’: lkey.y}

where stuID is your student ID, h and s are signature tuple and lkey.x and lkey.y are the <em>x </em>and <em>y </em>and coordinates of your long-term public key, respectively. A sample message is given in ‘samples.txt’.

<ol start="2">

 <li>If your message is verified by the server successfully, you will receive an e-mail, which includes your ID, your public key and a verification code: code.</li>

 <li>If your public key is correct in the verification e-mail, you will send another message to the server to authenticate yourself. The message format is “{‘ID’: stuID, ‘CODE’: code}”, where code is verification code which, you have received in the previous step. A sample message is given below.</li>

</ol>

{‘ID’: 18007, ‘CODE’: 209682}

<ol start="4">

 <li>If you send the correct verification code, you will receive an acknowledgement message via e-mail, which states that you are registered with the server successfully.</li>

</ol>

Once you register with the server successfully, you are not required to perform registration step again as the server stores your long-term public key to identify you. <strong>You need to store your long-term key pair as you will use them until the end of the project.</strong>

<h2>2.2       Station-to-Station Protocol</h2>

Here, you will develop a python code to implement the STS protocol. For the protocol, you will need the elliptic curve digital signature algorithm described in Section 2.3. The protocol has seven steps as explained below.

<ol>

 <li>You are required to generate an ephemeral key pair <em>s<sub>A </sub></em>and <em>Q<sub>A</sub></em>, which denote private and public keys, respectively. The key generation is described in “Key generation” algorithm in Section 2.3</li>

</ol>

Then, you will send a message, which includes your student ID and the ephemeral public key, to the server. The message format is “{‘ID’: stuID, ‘EKEY.X’: ekey.x, ‘EKEY.Y’: ekey.y”, where ekey.x and ekey.y are the <em>x </em>the <em>y </em>coordinates of your ephemeral public key, respectively. A sample message is given in ‘samples.txt’.

<ol start="2">

 <li>After you send your ephemeral public key, you will receive the ephemeral public key <em>Q<sub>B </sub></em>of the server. The message format is “{‘SKEY.X’: skey.x, ‘SKEY.Y: skey.y}”, where x and skey.y denote the <em>x </em>and <em>y </em>coordinates of <em>Q<sub>B</sub></em>, respectively. A sample message is given in ‘samples.txt’.</li>

 <li>After you receive <em>Q<sub>B</sub></em>, you are required to compute the session key <em>K </em>as follows.

  <ul>

   <li><em>T </em>= <em>s<sub>A</sub>Q<sub>B</sub></em></li>

   <li><em>U </em>= {<em>x</em>||<em>T.y</em>||“<em>BeY ourselfNoMatterWhatTheySay</em>”}<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>, where <em>T.x </em>and <em>T.y </em>denote the <em>x </em>and <em>y </em>coordinates of <em>T</em>, respectively.</li>

   <li><em>K </em>= SHA3 256(<em>U</em>)</li>

  </ul></li>

</ol>

A sample for this step is provided in ‘samples.txt’.

<ol start="4">

 <li>After you compute <em>K</em>, you should create a message <em>W</em><sub>1</sub>, which includes your and the server’s ephemeral public keys, generate a signature <em>Sig<sub>A </sub></em>using <em>s<sub>L </sub></em>for the message <em>W</em><sub>1</sub>. After that, you should encrypt the signature using AES in the Counter Mode (AES-CTR). The required operations are listed below.

  <ul>

   <li><em>W</em><sub>1 </sub>= <em>Q<sub>A</sub>.x</em>||<em>Q<sub>A</sub>.y</em>||<em>Q<sub>B</sub>.x</em>||<em>Q<sub>B</sub>.y</em>, where <em>Q<sub>A</sub>.x</em>, <em>Q<sub>A</sub>.y</em>, <em>Q<sub>B</sub>.x </em>and <em>Q<sub>B</sub>.y </em>are the <em>x </em>and <em>y </em>coordinates of <em>Q<sub>A </sub></em>and <em>Q<sub>B</sub></em>, respectively.</li>

   <li>(<em>Sig<sub>A</sub>.s,Sig<sub>A</sub>.h</em>) = Sign<em><sub>s</sub></em><em><sub>L</sub></em>(<em>W</em><sub>1</sub>)</li>

   <li><em>Y</em><sub>1 </sub>= <em>E<sub>K</sub></em>(“<em>s</em>”||<em>Sig<sub>A</sub>.s</em>||“<em>h</em>”||<em>Sig<sub>A</sub>.h</em>)</li>

  </ul></li>

</ol>

Then, you should concatenate the 8-byte nonce Nonce<em><sub>L </sub></em>and <em>Y</em><sub>1 </sub>and send {Nonce<em><sub>L</sub></em>||<em>Y</em><sub>1</sub>} to the server. Note that, you should convert the ciphertext from byte array to integer. A sample for this step is provided in ‘samples.txt’.

<ol start="5">

 <li>If your signature is valid, the server will perform the same operations which are explained in step 4. It creates a message <em>W</em><sub>2</sub>, which includes the server’s and your ephemeral public keys, generate a signature <em>Sig<sub>B </sub></em>using <em>s<sub>SL</sub></em>, where <em>s<sub>SL </sub></em>is the long-term private key of the server. After that, it will encrypt the signature using AES-CTR.</li>

</ol>

<h3>• <em>W</em><sub>2 </sub>= <em>Q<sub>B</sub>.x</em>||<em>Q<sub>B</sub>.y</em>||<em>Q<sub>A</sub>.x</em>||<em>Q<sub>A</sub>.y</em>. (Note that, <em>W</em><sub>1 </sub>and <em>W</em><sub>2 </sub>are different.)</h3>

<ul>

 <li>(<em>Sig<sub>B</sub>.s,Sig<sub>B</sub>.h</em>) = Sign<em><sub>s</sub></em><em><sub>L</sub></em>(<em>W</em><sub>2</sub>)</li>

 <li><em>Y</em><sub>2 </sub>= <em>E<sub>K</sub></em>(“<em>s</em>”||<em>Sig<sub>B</sub>.s</em>||“<em>h</em>”||<em>Sig<sub>B</sub>.h</em>)</li>

</ul>

Finally, it will concatenate the 8-byte nonce Nonce<em><sub>SL </sub></em>to <em>Y</em><sub>2 </sub>and send {Nonce<em><sub>SL</sub></em>||<em>Y</em><sub>2</sub>} to you. After you receive the message, you should decrypt it and verify the signature. The signature verification algorithm is explained in Section 2.3. A sample for this step is provided in ‘samples.txt’.

<ol start="6">

 <li>Then, the server will send to you another message <em>E<sub>K</sub></em>(<em>W</em><sub>3</sub>) <a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a> where, <em>W</em><sub>3 </sub>= {Rand||Message}. Here, Rand and Message denote an 8-byte random number and a meaningful message, respectively. You need to decrypt the message, and obtain the meaningful message and the random number Rand. A sample for this step is provided in ‘samples.txt’.</li>

 <li>Finally, you will prepare a message <em>W</em><sub>4 </sub>= {(Rand+1)||Message} and send <em>E<sub>K</sub></em>(<em>W</em><sub>4</sub>) <a href="#_ftn3" name="_ftnref3"><sup>[3]</sup></a> to the server. Sample messages for this step is given below.</li>

</ol>

<em>W</em><sub>4 </sub>: 86987

<em>MessagetoServer </em>: 86987

<ol start="8">

 <li>If your message is valid, the server will send a response message as <em>E<sub>K</sub></em>(“<em>SUCCESSFUL</em>”||Rand + 2) <sup>2</sup></li>

</ol>

<h2>2.3           Elliptic Curve Digital Signature Algorithm (ECDSA)</h2>

Here, you will develop a Python code that includes functions for signing given any message and verifying the signature. For ECDSA, you will use an algorithm, which consists of three functions as follows:

<ul>

 <li><strong>Key generation: </strong>A user picks a random secret key 0 <em>&lt; s<sub>A </sub>&lt; n </em>− 1 and computes the public key <em>Q<sub>A </sub></em>= <em>s<sub>A</sub>P</em>.</li>

 <li><strong>Signature generation: </strong>Let <em>m </em>be an arbitrary length message. The signature is computed as follows:

  <ol>

   <li><em>k </em>← Z<em>n</em>, (i.e., <em>k </em>is a random integer in [1<em>,n </em>− 2]).</li>

   <li><em>R </em>= <em>k </em> <em>P</em></li>

   <li><em>r </em>= <em>x </em>(mod <em>n</em>), where <em>R.x </em>is the <em>x </em>coordinate of <em>R</em></li>

   <li><em>h </em>= SHA3 256(<em>m </em>+ <em>r</em>) (mod <em>n</em>)</li>

   <li><em>s </em>= (<em>s<sub>A </sub></em> <em>h </em>+ <em>k</em>) (mod <em>n</em>)</li>

   <li>The signature for <em>m </em>is the tuple (<em>h,s</em>).</li>

  </ol></li>

 <li><strong>Signature verification: </strong>Let <em>m </em>be a message and the tuple (<em>s,h</em>) is a signature for <em>m</em>. The verification proceeds as follows:

  <ul>

   <li><em>V </em>= <em>sP </em>− <em>hQ<sub>A</sub></em></li>

   <li><em>v </em>= <em>x </em>(mod <em>n</em>), where <em>V.x </em>is x coordinate of <em>V</em></li>

   <li><em>h</em><sup>0 </sup>= SHA3 256(<em>m </em>+ <em>v</em>) (mod <em>n</em>)</li>

   <li>Accept the signature only if <em>h </em>= <em>h</em><sup>0 </sup><strong>– </strong>Reject it otherwise.</li>

  </ul></li>

</ul>

Note that the signature generation and verification of this ECDSA are different from the one discussed in the lecture.

<h1>3 Phase II: Developing software for receiving messages from other clients</h1>

In this phase of the project, you are required to upload one file: “Client phase2.py”. You will be provided with “Client basic phase2.py”, which includes all required communication codes. Moreover, you will find sample outputs for this phase in ‘sample vector.txt’, which is also provided on SUCourse.

You are required to develop a software for downloading 5 messages from the server, which were uploaded to the server originally by a pseudo-client, which is implemented by us, in this phase<a href="#_ftn4" name="_ftnref4"><sup>[4]</sup></a>. The details are given below.

In Phase I, you have already implemented the registration protocol. The details are explained in Section 2.1. If you have not implemented yet, you must implement the protocol before Phase II and register your long-term public key with the server.

<h2>3.1      Registration of ephemeral keys</h2>

Before communicating with other clients, you must generate 10 ephemeral public and private key pairs, namely (<em>s<sub>A</sub></em><sub>0</sub><em>,Q<sub>A</sub></em><sub>0</sub>)<em>,</em>(<em>s<sub>A</sub></em><sub>1</sub><em>,Q<sub>A</sub></em><sub>1</sub>)<em>,</em>(<em>s<sub>A</sub></em><sub>2</sub><em>,Q<sub>A</sub></em><sub>2</sub>)<em>,…,</em>(<em>s<sub>A</sub></em><sub>9</sub><em>,Q<sub>A</sub></em><sub>9</sub>)<em>,</em>, where <em>s<sub>A</sub></em><em><sub>i </sub></em>and <em>Q<sub>A</sub></em><em><sub>i </sub></em>denote your <em>i<sup>th </sup></em>private and public ephemeral keys, respectively. The key generation is described in “Key generation” algorithm in Section 2.3.

Then, you must sign each of your public ephemeral key using your long-term private key. The signatures must be generated for concatenated form of the ephemeral public keys (<em>Q<sub>A</sub></em><em><sub>i</sub>.x</em>||<em>Q<sub>A</sub></em><em><sub>i</sub>.y</em>). Finally, you must sent your ephemeral public keys to the server in the form of

{‘ID’: stuID, ‘KEYID’:i , ‘QAI.X’: QAi.x, ‘QAI.Y’: QAi.y, ‘SI’: si, ‘HI’: hi},

where i is the ID of your ephemeral key. You must start generating your ephemeral keys with IDs from 0 and follow the order. Moreover, you <strong>have to store </strong>your ephemeral private keys with their IDs.

<h3>3.1.1         Resetting the ephemeral keys</h3>

If you forget to store your ephemeral private keys or require to get new messages sent by the pseudo-client, you must sign your ID using your long-term private key and send a message to the server in the form of

{‘ID’: stuID, ‘S’: s, ‘H’: h }.

When the server receives your message, your ephemeral keys will be deleted. After you produce as new set of ephemeral keys and register with the server again, pseudo client will produce a new set of 5 messages for you.

<h2>3.2      Receiving messages</h2>

As mentioned above, you will download 5 messages from the server. In order to download one message from the server, you must sign your ID using your long-term private key and send a message to the server in the form of

{‘ID’: stuID, ‘S’: s, ‘H’: h }

to download one message from the server as follows

{‘IDB’: stuIDB, ‘KEYID’: i, ‘MSG’: msg , ‘QBJ.X’: QBj.x ,‘QBJ.Y’: QBj.y },

where stuIDB is the ID of the sender, i is the ID of your ephemeral key, which is used to generate session keys, msg contains both the encrypted message and its MAC, and QBj.x and QBj.y are <em>x </em>and <em>y </em>coordinates of the ephemeral public key of the server, respectively.

<h3>3.2.1         Session Key and msg Generation</h3>

As mentioned above, the message that you received includes the ciphertext as well as its MAC, which is concatenated to the end of the ciphertext. In order to create a message in this way, the pseudo-client will compute two session keys <em>K<sub>AB</sub><sup>ENC </sup></em>and <em>K<sub>AB</sub><sup>MAC </sup></em>using your and its ephemeral keys which are <em>Q<sub>A</sub></em><em><sub>i </sub></em>and <em>Q<sub>B</sub></em><em><sub>j</sub></em>, respectively. Before the computation, the pseudo-client requests your ephemeral key from the server and the server sends your ephemeral key <em>Q<sub>A</sub></em><em><sub>i </sub></em>with your key ID <em>i</em>. Then, it computes the session keys as follows:

<ul>

 <li><em>T </em>= <em>s<sub>B</sub></em><em><sub>j</sub>Q<sub>A</sub></em><em><sub>i</sub></em>, where <em>s<sub>B</sub></em><em><sub>j </sub></em>is the <em>j<sup>th </sup></em>secret ephemeral key of the pseudo-client.</li>

 <li><em>U </em>= {<em>x</em>||<em>T.y</em>||“<em>NoNeedToRunAndHide</em>”} <a href="#_ftn5" name="_ftnref5"><sup>[5]</sup></a></li>

 <li><em>K<sub>AB</sub><sup>ENC </sup></em>= SHA3 256(<em>U</em>)</li>

</ul>

<h3>• <em>K</em><em>ABMAC </em>= SHA3 256(<em>K</em><em>ABENC</em>)</h3>

After it computes the session keys, it encrypts the message with <em>K<sub>AB</sub><sup>ENC </sup></em>using AES-CTR<a href="#_ftn6" name="_ftnref6"><sup>[6]</sup></a> and computes HMAC-SHA256 <a href="#_ftn7" name="_ftnref7"><sup>[7]</sup></a>. of the ciphertext with <em>K<sub>AB</sub><sup>MAC</sup></em>

<h4>3.2.2        Decrypting the messages</h4>

After you download a message, which is sent by the pseudo-client, from the server, you must generate session keys firstly. As mentioned above, <em>Q<sub>B</sub></em><em><sub>j </sub></em>and <em>i </em>are given to you in the message. Therefore, you must compute <em>K<sub>AB</sub><sup>ENC </sup></em>and <em>K<sub>AB</sub><sup>MAC </sup></em>as <em>s<sub>A</sub></em><em><sub>i</sub>Q<sub>B</sub></em><em><sub>j </sub></em>and SHA3 256(<em>K<sub>AB</sub><sup>ENC</sup></em>), respectively. Then, you must verify the HMAC code and decrypt the message. Finally, you must send the decrypted message with your ID as follows

{‘ID’: stuID, ‘DECMSG’: decmsg}. where decmsg is the decrypted message.

<h1>4           Phase III: Developing software to communicate with other clients</h1>

In this phase of the project, you are required to upload one file: “Client phase3.py”. You will be provided with “Client basic phase3.py”, which includes all required communication codes. Moreover, you will find sample outputs for this phase in ‘sample vector.txt’, which is also provided on SUCourse.

For testing purpose, you may send message to your groupmate and s/he may receive your message, if you are working in groups of two. Note that, if you have not registered with the server yet, you must register. The details are given in Section 2.1. If you are working alone, we will provide a dummy user for you.

In this phase of the project you will work on two parts. In the first part, you will develop a software to send message to other users.(You are not limited to send message to your group-mate or dummy user. You may send everybody who takes CS411/507.). In second part, you will manage your ephemeral keys. The details are given in the subsequent sections.

<h2>4.1      Sending Messages</h2>

In this part of Phase III, you are required to develop a software to send messages to other clients. Firstly, you are required to generate session keys. The details of session key generation is given in Section 3.2.1. In order to generate session keys to send a message to another client, ephemeral public key of the receiver is needed. Therefore, you must request an ephemeral key of the receiver from the server. The message format is given below.

{‘IDA’: stuIDA, ‘IDB’: stuIDB, ‘S’: s, ‘H’: h }

where stuIDA and stuIDB are your and the receiver’s IDs, respectively. s and h is a signature tuple which generated for stuIDB using your long-term private key. If your request is valid, the server will send the ephemeral key of the receiver as follows:

{‘I’: i, ‘J’: j, ‘QBJ.x’: OBj.x, ‘QBJ.y’: OBj.y, }

where i and j denote your and the receiver’s ephemeral key IDs, respectively. Moreover, OBj.x and OBj.x are x and y coordinated of the <em>j<sup>th </sup></em>ephemeral key of the receiver, <em>Q<sub>B</sub></em><em><sub>j</sub></em>. You must generate the session key using is your <em>i<sup>th </sup></em>ephemeral key, <em>s<sub>A</sub></em><em><sub>i</sub></em>, and <em>Q<sub>B</sub></em><em><sub>j </sub></em>as follows:

<ul>

 <li><em>T </em>= <em>s</em><em>A</em><em>iQ</em><em>B</em><em>j</em></li>

 <li><em>U </em>= {<em>x</em>||<em>T.y</em>||“<em>NoNeedToRunAndHide</em>”}</li>

 <li><em>K<sub>AB</sub><sup>ENC </sup></em>= SHA3 256(<em>U</em>)</li>

</ul>

<h3>• <em>K</em><em>ABMAC </em>= SHA3 256(<em>K</em><em>ABENC</em>)</h3>

Then you must encrypts your message with <em>K<sub>AB</sub><sup>ENC </sup></em>using AES-CTR and compute HMAC-SHA256 of the ciphertext with <em>K<sub>AB</sub><sup>MAC</sup></em>. Finally, you must concatenate the nonce, ciphertext and MAC of the ciphertext and send to the server as follows:

{‘IDA’: stuIDA, ‘IDB’: stuIDB, ‘I’: i, ‘J’: j, ‘MSG’: msg } where msg includes the nonce, ciphertext and MAC of the ciphertext.

<h2>4.2      Status Control</h2>

As explained in several sections, the protocol which you are implementing in this project is an offline protocol. In other words, the messages which are sent by the sender are stored in the server until the receiver requests to get his/her messages. Moreover, ephemeral keys, which are generated by the sender and receiver, are used once (for only one message) to generate a new session key pair for encryption and MAC. Therefore, you are required to check the server for how many new messages you have and how many ephemeral keys are remaining. Moreover, you must generate new ephemeral keys and register with the server if needed. In order to be informed for new messages and remaining ephemeral keys, you must send your ID to the server as follows:

{‘ID’: stuID, ‘S’: s, ‘H’: h }

where s and h is a signature tuple which is generated for stuID using your long-term private key. If your request is valid, the server will send the number of new messages and remaining ephemeral keys. You may either get your messages from the server or

<h3>4.2.1         Registration of new ephemeral keys</h3>

In Phase II, you have generated 10 ephemeral keys and registered with the server and each ephemeral key is used for one message. On the other hand, you may send/receive more than 10 messages. Therefore, if some of your ephemeral keys are used, you must generate new ephemeral keys to cover to 10 and register with the server again. The registration protocol is explained in section 3.1.

On the other hand, more than 10 messages may be sent to a client, when s/he is offline. In this case, his/her last ephemeral key will be used more than once. For instance, if 12 messages is sent to an offline client, his/her first 9 ephemeral keys will be used for the first 9 messages and his/her 10<em><sup>th </sup></em>ephemeral key will be used for last 3 messages. As mentioned in section 3.1, you must follow the order for key IDs.

<h2>4.3       Sending message to pseudo-client for grading</h2>

For Phase III, your work will be graded automatically. After you develop the software, which is explained in Section 4.1 and Section 4.2, you are required to send the following quotes<a href="#_ftn8" name="_ftnref8"><sup>[8]</sup></a> to pseudoclient, whose ID is 18007. Quotes are:

<ol>

 <li>“The world is full of lonely people afraid to make the first move.” Tony Lip</li>

 <li>“I don’t like sand. It’s all coarse, and rough, and irritating. And it gets everywhere.” Anakin Skywalker</li>

 <li>“Hate is baggage. Life’s too short to be pissed off all the time. It’s just not worth it.” Danny Vinyard</li>

 <li>“Well, sir, it’s this rug I have, it really tied the room together.” The Dude</li>

</ol>

“Love is like taking a dump, Butters. Sometimes it works itself out. But sometimes, you need to give it a nice hard slimy push.” Eric Theodore Cartman

<a href="#_ftnref1" name="_ftn1">[1]</a> <a href="https://www.youtube.com/watch?v=d27gTrPPAyk">https://www.youtube.com/watch?v=d27gTrPPAyk</a>

<a href="#_ftnref2" name="_ftn2">[2]</a> The first 8-byte of message, which you receive in this step, is nonce.

<a href="#_ftnref3" name="_ftn3">[3]</a> All encrypted messages must be generated using unique nonce values. For each encryption, you must use AES.new(). Then, you must concatenate nonce and ciphertext.

<a href="#_ftnref4" name="_ftn4">[4]</a> This is indeed an asynchronous messaging application, whereby other users can send you a message even if you are not online. Yes, exactly like WhatsApp application.

<a href="#_ftnref5" name="_ftn5">[5]</a> <a href="https://www.youtube.com/watch?v=u1ZoHfJZACA">https://www.youtube.com/watch?v=u1ZoHfJZACA</a>

<a href="#_ftnref6" name="_ftn6">[6]</a> For encryption, AES-CTR will be used in this project

<a href="#_ftnref7" name="_ftn7">[7]</a> Note that, in this operation, SHA256 is used instead of SHA3-256

<a href="#_ftnref8" name="_ftn8">[8]</a> Note that, you must send only quotes without “ ” and the names of sayers.