pyjama
======
pyjama is a personal cloud software that provides file storage, email storage and various related services. It is implemented in pure java and based on components that are well-tested and widely used in enterprise software.

Targets:

- pure java
- runs everywhere without installation. optional per-OS features like install-as-service. consider implementing NAT traversal.
- zip, move to other box, unzip, run
- modular, abstract: Instead of maintaining components forever, they should communicate through well-defined interfaces and should be easy to replace. (Why GWT then?)
- configure if you want. reasonable defaults, add configurability when needed.
- secure. outside and inside. build like operating system:
    - require codebase or subject domain for repository operations. proper handling of authentication should be a good foundation.
    - allow plugging of custom repositories or file systems into repository.
- handle data with care. make backups easy and automatic if possible.
- can act as proxy: provide every service that is used.
    - mount WebDAV into local JCR and expose via WebDAV
    - fetch email via IMAP and expose via IMAP
 
Components:

- JCR: The base for pyjama is a Java Content Repository. The default implementation will be Jackrabbit, but this should remain configurable. This relation should be similar to the relation between an operating system and a file system.
    - Mount remote JCR/WebDAV into local JCR. Jackrabbit has its own WebDAV client and this should be doable.
- JAAS: Will manage access to all services. Default implementation should be custom, using the repository as user database.
- Jetty: Will be default web server, but any other should work just fine.
- WebDAV: For every user, a WebDAV folder will be made visible on top of the JCR. Jackrabbit will take care of this.
    - CalDAV/etc: research.
- Email server: James-Mailbox in JCR.
    - Fetchmail for remote Pop3/IMAP
    - IMAP server: expose local mailbox via IMAP
    - email web application: James-HUPA will do this in GWT
- GWT for web GUI. Problem: what if we don't like GWT anymore?
- FTP? probably not.
- UPnP media server? UPnP remote? What about multiple users? Proxy will not make sense here.
