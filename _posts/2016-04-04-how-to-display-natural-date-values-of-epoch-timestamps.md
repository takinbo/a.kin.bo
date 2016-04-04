---
title: How to display natural date values of epoch timestamps
date: 2016-04-04 16:16:52 +0100
---

Every now and then I come across a unix timestamp that I'm interested in quickly estimating what date value it is.

After writing numerous throwaway scripts to do this, I decided to find a way to write a bash function so I can do this anytime I come across a timestamp again.

On Linux (and possibly other UNIX OSes) simply run this or put it in your `.bashrc` or `.bash_profile`

```bash
function epoch() { date -d @$1 }
```

On OSX the date utility works a bit different:

```bash
function epoch() { date -jf '%s' $1 }
```

Now whenever you want to convert a timestamp into a date value simply type `epoch` followed by the timestamp value and hit the return/enter key.

On 32-bit systems, you will get bit by the [Year 2038 problem](https://en.wikipedia.org/wiki/Year_2038_problem) so beware of that.