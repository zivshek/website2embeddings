# DTLSServer

Inherits: RefCounted < Object

Helper class to implement a DTLS server.

## Description

This class is used to store the state of a DTLS server. Upon setup() it
converts connected PacketPeerUDP to PacketPeerDTLS accepting them via
take_connection() as DTLS clients. Under the hood, this class is used to store
the DTLS state and cookies of the server. The reason of why the state and
cookies are needed is outside of the scope of this documentation.

Below a small example of how to use it:

GDScriptC#

    
    
    # server_node.gd
    extends Node
    
    var dtls = DTLSServer.new()
    var server = UDPServer.new()
    var peers = []
    
    func _ready():
        server.listen(4242)
        var key = load("key.key") # Your private key.
        var cert = load("cert.crt") # Your X509 certificate.
        dtls.setup(TlsOptions.server(key, cert))
    
    func _process(delta):
        while server.is_connection_available():
            var peer = server.take_connection()
            var dtls_peer = dtls.take_connection(peer)
            if dtls_peer.get_status() != PacketPeerDTLS.STATUS_HANDSHAKING:
                continue # It is normal that 50% of the connections fails due to cookie exchange.
            print("Peer connected!")
            peers.append(dtls_peer)
    
        for p in peers:
            p.poll() # Must poll to update the state.
            if p.get_status() == PacketPeerDTLS.STATUS_CONNECTED:
                while p.get_available_packet_count() > 0:
                    print("Received message from client: %s" % p.get_packet().get_string_from_utf8())
                    p.put_packet("Hello DTLS client".to_utf8_buffer())
    
    
    
    // ServerNode.cs
    using Godot;
    
    public partial class ServerNode : Node
    {
        private DtlsServer _dtls = new DtlsServer();
        private UdpServer _server = new UdpServer();
        private Godot.Collections.Array<PacketPeerDtls> _peers = [];
    
        public override void _Ready()
        {
            _server.Listen(4242);
            var key = GD.Load<CryptoKey>("key.key"); // Your private key.
            var cert = GD.Load<X509Certificate>("cert.crt"); // Your X509 certificate.
            _dtls.Setup(TlsOptions.Server(key, cert));
        }
    
        public override void _Process(double delta)
        {
            while (_server.IsConnectionAvailable())
            {
                PacketPeerUdp peer = _server.TakeConnection();
                PacketPeerDtls dtlsPeer = _dtls.TakeConnection(peer);
                if (dtlsPeer.GetStatus() != PacketPeerDtls.Status.Handshaking)
                {
                    continue; // It is normal that 50% of the connections fails due to cookie exchange.
                }
                GD.Print("Peer connected!");
                _peers.Add(dtlsPeer);
            }
    
            foreach (var p in _peers)
            {
                p.Poll(); // Must poll to update the state.
                if (p.GetStatus() == PacketPeerDtls.Status.Connected)
                {
                    while (p.GetAvailablePacketCount() > 0)
                    {
                        GD.Print($"Received Message From Client: {p.GetPacket().GetStringFromUtf8()}");
                        p.PutPacket("Hello DTLS Client".ToUtf8Buffer());
                    }
                }
            }
        }
    }
    

GDScriptC#

    
    
    # client_node.gd
    extends Node
    
    var dtls = PacketPeerDTLS.new()
    var udp = PacketPeerUDP.new()
    var connected = false
    
    func _ready():
        udp.connect_to_host("127.0.0.1", 4242)
        dtls.connect_to_peer(udp, false) # Use true in production for certificate validation!
    
    func _process(delta):
        dtls.poll()
        if dtls.get_status() == PacketPeerDTLS.STATUS_CONNECTED:
            if !connected:
                # Try to contact server
                dtls.put_packet("The answer is... 42!".to_utf8_buffer())
            while dtls.get_available_packet_count() > 0:
                print("Connected: %s" % dtls.get_packet().get_string_from_utf8())
                connected = true
    
    
    
    // ClientNode.cs
    using Godot;
    using System.Text;
    
    public partial class ClientNode : Node
    {
        private PacketPeerDtls _dtls = new PacketPeerDtls();
        private PacketPeerUdp _udp = new PacketPeerUdp();
        private bool _connected = false;
    
        public override void _Ready()
        {
            _udp.ConnectToHost("127.0.0.1", 4242);
            _dtls.ConnectToPeer(_udp, validateCerts: false); // Use true in production for certificate validation!
        }
    
        public override void _Process(double delta)
        {
            _dtls.Poll();
            if (_dtls.GetStatus() == PacketPeerDtls.Status.Connected)
            {
                if (!_connected)
                {
                    // Try to contact server
                    _dtls.PutPacket("The Answer Is..42!".ToUtf8Buffer());
                }
                while (_dtls.GetAvailablePacketCount() > 0)
                {
                    GD.Print($"Connected: {_dtls.GetPacket().GetStringFromUtf8()}");
                    _connected = true;
                }
            }
        }
    }
    

## Methods

Error | setup(server_options: TLSOptions)  
---|---  
PacketPeerDTLS | take_connection(udp_peer: PacketPeerUDP)  
  
## Method Descriptions

Error setup(server_options: TLSOptions)

Setup the DTLS server to use the given `server_options`. See
TLSOptions.server().

PacketPeerDTLS take_connection(udp_peer: PacketPeerUDP)

Try to initiate the DTLS handshake with the given `udp_peer` which must be
already connected (see PacketPeerUDP.connect_to_host()).

Note: You must check that the state of the return PacketPeerUDP is
PacketPeerDTLS.STATUS_HANDSHAKING, as it is normal that 50% of the new
connections will be invalid due to cookie exchange.

## User-contributed notes

Please read the User-contributed notes policy before submitting a comment.

* * *

Built with Sphinx using a theme provided by Read the Docs.

