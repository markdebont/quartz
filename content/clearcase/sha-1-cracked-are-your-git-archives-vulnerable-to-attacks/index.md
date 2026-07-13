---
archive_status: raw_import_enriched
categories:
- geen-categorie
chapter: Werk en IT
content_kind: post
date: 2015-10-20
draft: false
publish: true
review_note: Nog inhoudelijk beoordelen vóór publicatie.
source_site: CLEARCASE.NL
source_type: wordpress_export
source_url: https://itsaclearcase.wordpress.com
tags:
- git-configuration-management-sha-1
themes:
- IT
- softwareontwikkeling
- vakontwikkeling
- persoonlijk archief
title: SHA-1 cracked, are your GIT archives vulnerable to attacks?
visibility: public
---

Recently it was announced that SHA-1 signatures are now susceptible to collision attacks ([source](https://sites.google.com/site/itstheshappening/)).

As GIT uses SHA-1 signatures to uniquely identify objects you might ask yourself if it is still safe to store objects in your GIT code archive.

Let's find out!

First, SHA-1 signatures are used in GIT to ensure data integrity and consistency to detect corruption by DRAM or disks etc.

However, it was never implemented as a security feature to keep attackers out. For that purpose other mechanism have been implemented in GIT: signed commits.

While in 2012 it was predicted it would take approx. $2.7M ([source](http://tweakers.net/nieuws/84797/succesvolle-collisionaanval-sha-1-in-zicht.html)) to break a single hash value it can now (fall 2015) be done with a budget of $100.000 by renting CPU from a cloud provider like Amazon EC2.

If you are curious how a SHA-1 hash of an object or text string looks like you can do this by executing the following line in a DOS command window:

|   ``` D:\>echo 'clearcase.nl' \| git hash-object --stdin a6ea340af55c1fc7071f0c6b50218ca6380d86cb ```     |
| --- |

Now, let's assume the attacker is attacking a remote repository.

When the attacker has access to a local repository there are probably much easier and cheaper ways to inject code into a GIT repository, so this is the logical scenario.

The attacker created a file containing malicious code with an identical SHA-1 and committed it to the remote repository.

Now what happens is that the new object will not be created!

Since the malicious file that the hacker wants to inject  has the same SHA-1 has the original file in your repository the commit or GIT index ends up pointing  to the old object.

GIT thinks you have committed the same object as it has an identical SHA-1 hash.

Bottom line is that SHA-1 collisions are not very relevant for existing GIT repositories. Use signed commit messages for extra security instead.