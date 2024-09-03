---
title: আর্চলিনাক্সে সিংকথিং ইন্সটল এবং সেটআপ
slug: syncthing-arch
date: 2024-09-03 15:00:00+0600
categories:
  - writeups
  - tutorial
tags:
  - writeups
  - syncthing
  - arch linux
  - tutorial
draft: false
---

## সিংকথিং (Syncthing)

বেশ কিছুদিন ধরেই একাধিক ডিভাইসের মধ্যে ফাইল সিংক্রোনাইজেশন নিয়ে সমস্যায় পড়েছিলাম। ঘাটাঘাটি করতে গিয়ে পেলাম Syncthing কে। দেখে ভালোই লাগলো, আজকে এ লেখায় তাই কিভাবে এটি আর্চলিনাক্সে সেটআপ করতে হয় তা লিখবো।

## ইন্সটলেশন

আর্চলিনাক্সের অফিসিয়াল প্যাকেজ রিপোজিটরিতে Syncthing এর প্যাকেজ আছে। তাই সহজেই ইন্সটল করা যাবে।

```bash
sudo pacman -Sy syncthing
```

## সেটআপ

ইন্সটলেশনের পর সবচেয়ে গুরুত্বপূর্ণ হলো সেটআপ। একটা পদ্ধতি হলো টার্মিনালে গিয়ে `syncthing` কমান্ড রান করে ব্রাউজারে [http://127.0.0.1:8384/](http://127.0.0.1:8384/) লিংক ভিজিট করে। স্বাভাবিক ভাবেই যতবার আমরা পিসি চালু করবো ততবার এই কমান্ডটি রান করতে হবে। এই সমস্যার সমাধান করতে ঘাটাঘাটি শুরু করলাম systemd এর সাথে কিভাবে syncthing সেটআপ করা যায়।

## সিস্টেমডিতে সেটআপ

1. সিস্টেমডির সাথে সেটআপ করতে প্রথমেই ইউজার তৈরি অথবা সিলেক্ট করে নিতে হবে। আমার ডিফল্ট করা আছে তাই স্কিপ করলাম। 😛
2. এরপর `syncthing@.service` ফাইলটি `/usr/lib/systemd/system/` ডিরেক্টরিতে আছে কিনা দেখে নিতে হবে। না থাকলে [গিটহাব](https://github.com/syncthing/syncthing/raw/main/etc/linux-systemd/system/) থেকে ডাউনলোড করে নিতে হবে।
3. এরপর `syncthing@.service` ফাইলটি এডিটর দিয়ে `[Service]` সেকশনের নিচে `User=` ডিরেক্টিভসটি এডিট করে ইউজারনেম দিয়ে নিতে হবে।

4. এরপর অটোস্টার্ট চালু করতে নিচের কমান্ডটি রান করতে হবে।

```bash
sudo systemctl enable syncthing@username.service
```

এখানে `@` এর পর `username` এর জায়গায় `User=` ডিরেক্টিভসের ইউজারনেমটি দিতে হবে।

5. সিস্টেম ইউনিটটি চালু করতে নিচের কমান্ডটি রান করতে হবে।

```bash
sudo systemctl start syncthing@username.service
```

এখানেও `@` এর পর `username` এর জায়গায় `User=` ডিরেক্টিভসের ইউজারনেমটি দিতে হবে।

6. সর্বশেষ ব্রাউজারে [http://127.0.0.1:8384/](http://127.0.0.1:8384/) লিংক ভিজিট করলে Syncthing এর ইউজার ইন্টারফেস দেখা যাবে।

## তথ্যসূত্র

1. [Official Syncthing Documentation](https://docs.syncthing.net/users/autostart.html#linux)
1. [AskUbuntu StackExchange](https://askubuntu.com/questions/676007/how-do-i-make-my-systemd-service-run-via-specific-user-and-start-on-boot)
