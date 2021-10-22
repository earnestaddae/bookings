{
email youremail address
}

(static) {
@static {
file
path _.ico _.css _.js _.gif _.jpg _.jpeg _.png _.svg _.woff _.json
}
header @static Cache-Control max-age=5184000
}

(security) {
header { # enable HSTS
Strict-Transport-Security max-age=31536000; # dissable clients from sniffing media type
X-Content-Type-Options nosniff # keep referrer data off of HTTP connections
Referrer-Policy no-referrer-when-downgrade
}
}

import conf .d/\*.conf
