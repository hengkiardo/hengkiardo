<!--
id: 26326449702
link: http://blog.hengkiardo.com/post/26326449702/compressing-and-caching-your-content-with-htaccess
slug: compressing-and-caching-your-content-with-htaccess
date: Mon Jul 02 2012 11:37:00 GMT+0700 (WIT)
publish: 2012-07-02
tags: htaccess, caching
title: Compressing and caching your content with .htaccess
-->


    <ifModule mod_gzip.c> 
      mod_gzip_on Yes
      mod_gzip_dechunk Yes
      mod_gzip_item_include file \.(html?|txt|css|js|php|pl)$
      mod_gzip_item_include handler ^cgi-script$
      mod_gzip_item_include mime ^text/.*
      mod_gzip_item_include mime ^application/x-javascript.*
      mod_gzip_item_exclude mime ^image/.*
      mod_gzip_item_exclude rspheader ^Content-Encoding:.*gzip.*
    </ifModule>

    # BEGIN Compress text files
    <ifModule mod_deflate.c>
      <filesMatch "\.(css|js|x?html?|php)$">
        SetOutputFilter DEFLATE
      </filesMatch>
    </ifModule>
    # END Compress text files
     
    # BEGIN Expire headers
    <ifModule mod_expires.c>
      ExpiresActive On
      ExpiresDefault "access plus 1 seconds"
      ExpiresByType image/x-icon "access plus 2592000 seconds"
      ExpiresByType image/jpeg "access plus 2592000 seconds"
      ExpiresByType image/png "access plus 2592000 seconds"
      ExpiresByType image/gif "access plus 2592000 seconds"
      ExpiresByType application/x-shockwave-flash "access plus 2592000 seconds"
      ExpiresByType text/css "access plus 604800 seconds"
      ExpiresByType text/javascript "access plus 216000 seconds"
      ExpiresByType application/javascript "access plus 216000 seconds"
      ExpiresByType application/x-javascript "access plus 216000 seconds"
      ExpiresByType text/html "access plus 600 seconds"
      ExpiresByType application/xhtml+xml "access plus 600 seconds"
    </ifModule>
    # END Expire headers
     
    # BEGIN Cache-Control Headers
    <ifModule mod_headers.c>
      <filesMatch "\.(ico|jpe?g|png|gif|swf)$">
        Header set Cache-Control "max-age=2592000, public"
      </filesMatch>
      <filesMatch "\.(css)$">
        Header set Cache-Control "max-age=604800, public"
      </filesMatch>
      <filesMatch "\.(js)$">
        Header set Cache-Control "max-age=216000, private"
      </filesMatch>
      <filesMatch "\.(x?html?|php)$">
        Header set Cache-Control "max-age=600, private, must-revalidate"
      </filesMatch>
    </ifModule>
    # END Cache-Control Headers

    # BEGIN Turn ETags Off
    <ifModule mod_headers.c>
      Header unset ETag
    </ifModule>
    FileETag None
    # END Turn ETags Off
     
    # BEGIN Remove Last-Modified Header
    <ifModule mod_headers.c>
      Header unset Last-Modified
    </ifModule>
    # END Remove Last-Modified Header
