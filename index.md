{:refdef: style="text-align: center;"}
![libssh logo](https://www.libssh.org/wp-content/uploads/2015/01/libssh1.png)
{: refdef}

# **Implementation of GSSAPI Key Exchange and Improve Testing**
{: style="text-align: center"}

## **Overview**

- Organization - [libssh](https://www.libssh.org/)

- Project Proposal - [link](https://drive.google.com/file/d/1G4qNzz-7Hw8SFSnJyOFrSLoP-bc4a2E-/view)

- Mentors

  - Jakub Jelen - [gitlab](https://gitlab.com/jjelen)

  - Sahana Prasad - [gitlab](https://gitlab.com/sahprasa)

  - Eshan Kelkar - [gitlab](https://gitlab.com/eshankelkar)

- Contributor

  - Gauravsingh Sisodia - [gitlab](https://gitlab.com/xaerru)


## **Goals**

1. Setting up Kerberos in a test environment

2. Implementing GSSAPI Key Exchange


## **1. Setting up Kerberos in a test environment**

- libssh supported the "gssapi-with-mic" authentication method but lacked automated test coverage for both client and server.

- Setup and teardown functions were created to configure the Kerberos KDC (Key Distribution Center) with different configurations.

- cwrap wrappers were used, and Kerberos environment variables were set up as needed.

- Tests were written for the libssh client to test the "gssapi-with-mic" authentication method against the OpenSSH server.

- libssh server was tested against the libssh client.

- Tests were added to check the delegation of credentials and verify libssh server callbacks.

- In addition to adding tests, memory leaks were identified and fixed, documentation and comments were added and server callbacks were configured properly.


## **2. Implementing GSSAPI Key Exchange**

- GSSAPI Key Exchange, as described in RFC 4462, was first implemented for the libssh client.

- This implementation was then tested with the OpenSSH server.

- GSSAPI Key Exchange was subsequently implemented for the libssh server.

- The server implementation was then tested with the libssh client.

- GSSAPI key exchange algorithms had to be handled separately because they require a suffix.

- During this process, generic GSSAPI functions were created to minimize code duplication.

- Options were added to both the client and server to enable GSSAPI Key Exchange and configure the key exchange algorithms.

- "gssapi-keyex" authentication method was implemented and tested for both the client and server.

- The `SSH2_MSG_KEXGSS_HOSTKEY` message, an optional feature not implemented in other SSH server implementations, was added to the libssh server, with corresponding handling implemented for the client.


## **Merge Requests**

- GSoC period

  - Testing for GSSAPI Authentication (Merged) - [link](https://gitlab.com/libssh/libssh-mirror/-/merge_requests/490)

  - Implementing GSSAPI Key Exchange (Merged) - [link](https://gitlab.com/libssh/libssh-mirror/-/merge_requests/505)

- Before GSoC period

  - Implement proxy jump using libssh (Merged) - [link](https://gitlab.com/libssh/libssh-mirror/-/merge_requests/459)

  - Handle hostkeys like OpenSSH (Merged) - [link](https://gitlab.com/libssh/libssh-mirror/-/merge_requests/452)

  - Fix example server, check for all keys in authorized\_keys file (Merged) - [link](https://gitlab.com/libssh/libssh-mirror/-/merge_requests/440)


## **Current Status**

- Testing for GSSAPI authentication has been merged and released in libssh 0.11.0

- gss-group14-sha256-\* and gss-group16-sha512-\* GSSAPI Diffie-Hellman Key Exchange algorithms have been implemented.

- "gssapi-keyex" authentication method has been implemented.

- Support "null" hostkey algorithm for both libssh client and server.

## **Challenges and Learnings**

- I often felt stuck but didn’t give up, eventually figuring things out either with the help of my mentors or on my own. Here are some challenges I faced:

    - Encountered memory leaks in the krb5 library, which required looking into the krb5 codebase to discover that the error strings were freed only after a call to `pthread_exit()`.

    - After the initial implementation of GSSAPI Key Exchange, I felt stuck when the encryption wasn't working. But after carefully reviewing the RFC, I realized that if the server host key was not found, the hostkey should not be omitted; instead, an empty string needed to be included in the hash buffer.

    - While testing the null host key algorithm on Fedora, I encountered a situation where the key exchange completed successfully but encryption failed. This issue did not occur on CentOS Stream 9, leading to the identification of a bug in Fedora's OpenSSH patches.

- Learnings

    - Learnt a lot about the SSH protocol, Kerberos and GSSAPI.

    - Improved C programming skills, learnt about how C is used in real-world codebases.

    - Learnt how to work in a team and also how to address code reviews.

    - Learnt how to present my work effectively in weekly meetings.


## **Acknowledgements**

- I’m extremely grateful to my mentors for accepting my proposal and guiding me throughout the project.

- Special thanks to my friends and family for their support and encouragement.
