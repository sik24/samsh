import socket
import traceback

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
s.setsockopt(socket.SOL_SOCKET, socket.SO_BROADCAST, 1)
s.bind(('', 2390))

print "Listening for broadcasts..."

while 1:
    try:
        message, address = s.recvfrom(8192)
        print "Got message from %s: %s" % (address, message)
        # verificare daca primeste de la SAMSH
        if address == "192.168.0.101":
            if message == "deapta":
                os.system("ulndr.py")
                s.sendto("Acknoleage", address)
            if message == "stanga":
                os.system("ulndr.py")
                s.sendto("Acknoleage", address)
        print "Listening for broadcasts..."
    except (KeyboardInterrupt, SystemExit):
        raise
    except:
        traceback.print_exc()
