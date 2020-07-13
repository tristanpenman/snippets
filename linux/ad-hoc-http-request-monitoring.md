# Ad-hoc HTTP request monitoring

At some point, I wanted to monitor the HTTP requests that were being received by a server, but couldn't trust the web server or some other aspect of the system. A simple approach to this is to use `tcpflow` combined with `grep`.

For example, to monitor basic HTTP requests on the loopback device:

    sudo tcpflow -p -c -i lo port 80 | grep -oE '(GET|POST|HEAD) .* HTTP/1.[01]|Host: .*'

Running this while handling some requests to a WordPress site yields output that looks like this:

    GET / HTTP/1.1
    GET /wp-includes/css/dashicons.min.css?ver=5.4.2 HTTP/1.1
    GET /wp-includes/css/admin-bar.min.css?ver=5.4.2 HTTP/1.1
    GET /wp-includes/css/dist/block-library/style.min.css?ver=5.4.2 HTTP/1.1
    GET /wp-content/plugins/buddypress/bp-members/css/blocks/member.min.css?ver=6.1.0 HTTP/1.1
    ...
