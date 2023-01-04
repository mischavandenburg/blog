---
date: "2022-09-13"
tags:
- Study
- DevOps
title: Certified Kubernetes Administrator (CKA) Exam Tips
---
I recently obtained my CKA certification. I started this certification journey with zero knowledge of Kubernetes. However, I was already working as a DevOps Engineer, and I know a fair bit of Linux. I daily drive Arch Linux and have LPIC-1 certification. It was handy to know where files are located on Linux systems and how to interact with systemd services. I also knew yaml quite well because I work with Ansible daily. I passed on my first try, and I did one session of killer.sh.

### My preparation
- [KodeKloud CKA Course](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/)
- [Killer.sh Mock Exam](https://killer.sh/)
- [Killercoda](https://killercoda.com/killer-shell-cka)

I kept track of the time I spent on this certification. In total, I spent 80 hours on study and practice.

### In Hindsight

I spent too much time repeating things during the KodeKloud course. This is the one thing I would do differently if I could start over. I went over some modules multiple times and kept meticulous notes. However, I have hardly used any of those notes. But they will be nice to have for the future.

I learned most from the killer.sh exams. So I would advise you to go through the KodeKloud course and do all the exercises, but don't spend too much time repeating stuff. If you don't understand the topic at all, it is, of course, necessary to repeat it. But you don't need to know all the details.

### Killer.sh

After I finished the KodeKloud course, I purchased the exam voucher and started the killer.sh on Saturday morning. I wanted to simulate the exam experience as much as possible, so I set the timer and did not allow myself to stand up for two hours. My first round was humiliating. I only managed to get 24 out of 125 points. A little shocked by the experience, I spent the whole Saturday going through all the solutions of the exercises that killer.sh provides. The explanations they give are extensive, and I found them helpful. Saturday evening, I went out for dinner with friends, and on Sunday morning, I passed killer.sh. I spent the whole Sunday studying the solutions more and more, and on my last try on Sunday evening, I scored 115 out of 125.

## Tips:
- I know tmux quite well and used it extensively during the killer.sh, but it was not necessary during the exam. No need to learn it if you don't know it already.
- Knowing vim well will save you a lot of time at the exam. For example, dG to delete all lines until the end of the file from your current location. Run "vimtutor" on a Linux system to learn the basics.
- You cannot use bookmarks. Learn how to search the docs efficiently. One handy one I figured out was to control + F and enter "kind: Pod" or "kind: PersistentVolume" to immediately go to the example YAML.
- my exam environment did not need much extra configuration. All I added to my .bashrc was alias v=vim and export do="--dry-run=client -o yaml" so you can use "k run Nginx $do > Nginx.YAML"
- The exam environment is not as bad as people make it out to be on the internet. There is a little delay while scrolling through the docs in the browser, but working in the terminal didn't give me any problems. Get used to the environment on killer.sh, and there should not be any surprises in the real exam environment.
- Skip questions you cannot solve immediately. But don't spend time reviewing all the questions, sorting by the highest % and doing those first. You will lose a lot of time evaluating all of these questions. It is much better to solve the questions during your first pass through and skip the ones you cannot immediately solve.
- When the 120-minute timer ran out, I was presented with a screen that said "quit" or "request more time." I was pretty sure I could not get more time for this exam, so I just pressed "quit." After I pressed quit, the application closed immediately, and there was no confirmation whatsoever that they received my exam results or anything. This was extremely disorienting, and I was left doubting if I had done it correctly. Eventually, I could see in the Linux Foundation portal that my exam was in Grading status.
- Speed is of the essence. An hour before my exam, I used killercoda to get into the mood and get things up to speed. Learn to solve things quickly and don't spend time having to arrange terminal windows on your screen or stumbling around in vim. You cannot afford to lose time on these things.

- Finally, this video is an excellent summary of all the necessary tips and information: [](https://www.youtube.com/watch?v=8VK9NJ3pObU)
