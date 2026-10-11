# Upstream's Dockerfile (XBEN-041-24/php/Dockerfile) with these changes:
# - the sources are copied from app/php/ (the vendored benchmark folder sits in app/)
# - the build argument FLAG defaults to this lab's fixed flag, the value upstream's Makefile passes
#   (FLAG{sha256 of "XBEN-041-24"}), since the spec passes no build arguments
# - on a Debian buster or bullseye base, apt reads archive.debian.org: both releases have left (or
#   are leaving) the Debian mirrors, so upstream's apt-get install fails there
FROM php:7.1-apache
RUN . /etc/os-release && case "$VERSION_CODENAME" in \
      buster) sec=buster/updates ;; bullseye) sec=bullseye-security ;; *) sec= ;; esac \
 && if [ -n "$sec" ]; then \
      printf 'deb http://archive.debian.org/debian %s main\ndeb http://archive.debian.org/debian-security %s main\n' \
        "$VERSION_CODENAME" "$sec" > /etc/apt/sources.list && rm -f /etc/apt/sources.list.d/*; fi

RUN apt-get update \
    && apt-get install -y --no-install-recommends curl \
    && rm -rf /var/lib/apt/lists/*

ARG FLAG=FLAG{3ea5dcbf70e8634b108b5200dc6a6dee53ef1cdc2581e350aa2fa726ad09e9ae}
ENV FLAG=$FLAG

COPY app/php/challenge /var/www/html/
COPY app/php/apache/000-default.conf /etc/apache2/sites-available/000-default.conf

RUN mkdir /var/www/html/upload
RUN chown -R www-data:www-data /var/www/html

WORKDIR /var/www/html

EXPOSE 80

# CyberCTF per-player flag: the flag above is only the development fallback. ctf-entrypoint.sh
# puts the player's flag (CTF_FLAG_MAIN) in its place at every start, then runs the original
# entrypoint and command, repeated here unchanged.
COPY ctf-entrypoint.sh /usr/local/bin/ctf-entrypoint.sh
ENTRYPOINT ["/usr/local/bin/ctf-entrypoint.sh", "docker-php-entrypoint"]
CMD ["apache2-foreground"]
