<a name="page-top"></a>
# UMG-Slate Compendium FAQ(Frequently Asked Questions)

All source code referenced in the compendium is from the launcher and Epic's public github repository of Unreal Engine.

*Any major updates that the author of the document(also owner of the repository) have to be confirmed by the author's place of business to make sure no "trade secrets" are revealed, 
this can be a time consuming process BUT it helps avoid an legal issues that the document may incur.
Pull Requests from the Community are not subject to this rule and can immediately be merged into the public repository if its approved.*

---

<a name="repo-page-links"></a>
## Repository Page Links

> - [Main Compendium Page](README.md)
> - [External Links Page](EXTERNAL_LINKS.md)

---

<a name="translation-localization-faq"></a>
## Translation/Localization FAQ

For translating the compendium and its documents we use [GitLocalize](https://gitlocalize.com/). \
For requesting to become a translator please fill out this form([LINK](https://forms.gle/xyS4otdTTNHLKfYz9)),
if you already submitted the form and want to localize a different language(or additional one) you can just edit the form and re-submit it.

Languages the Compendium is currently localized in(including how many translators for that language):
- English(Original Language)
   - 1 Translator
- Russian
   - 1 Translator
- Chinese
   - 1 Translator

<a name="questions"></a>
## Questions

---
1. QUESTION: I found an issue I wanted to report, what's the next steps? \
ANSWER: Please go to the "Issues" tab of this GitHub repository and make a new issue for that, 
I have a template for certain types of issues but the general format is to include: 
   - What the issue is.
   - Where its located(including the section number is extremely helpful so the author knows that you're referring to).
   - `-OPTIONAL & RECOMMENDED-` Suggesting a fixed version(if applicable) to help get the fix in quicker(the templates will explicitly say whether you should have a suggestion or not).
---
2. QUESTION: I want to help contribute to this document! How can I do that? \
ANSWER: You can make a fork of the repository and then make a pull request to merge your contribution(s) into this repository like if you were contributing to Unreal Engine. You are required that you have written permission from your place of business to contribute(it can be in any forms but it needs to be in written form as proof sort of thing), this avoid any legal issues regarding "trade secrets" being revealed but even then if the information is referring to the Launcher/public Github repository of Unreal Engine then it shouldn't be an issue for them to approve it.
---
3. QUESTION: What's the policy regarding Pull Requests? \
ANSWER: 
    - Anybody can make a pull request to make changes to the compendium!
    - You must be polite and respectful to others(you get 3 chances but depending on severity it may be less, use your best judgement obviously). We're all using Unreal Engine, somebody may be more experienced with it but that does not give you the right to be a jerk and vise versa.
    - All information referring to Unreal Engine must come from the Launcher or public Github repository of Unreal Engine, NOT private repositories from either your company or from Epic's own private repository.
    - If you work at a business that uses Unreal Engine(or something related to it), I would prefer if you have written(NOT VERBAL) permission from that business that you can contribute to the project even if its a one-off contribution(a slack message or email from your boss saying its fine is enough for me). This is to avoid legal issues regarding revealing "trade secrets", but even then all of the information in the compendium refers to public knowledge so it shouldn't be an issue for them to approve it.
---
4. QUESTION: What is the process of Pull Requests? \
ANSWER: 
    1. Fork the repository.
    2. Create a branch on your forked repository for your changes to be encapsulated and contained to that branch.
    3. Make your changes in your branch of your forked repository.
    4. When your ready to bring it into this repository, create a pull request from your branch to be merged into the branch named: `main` of this repository.
    5. Fill out the pull request with a detailed description of your changes, **DO NOT add any reviewers!** the maintainers of this repository will do this. **DO NOT @ anybody that hasn't commented on the pull request already, basically be respectful of the maintainers time, we will get to it!**.
    6. A maintainer will comment with any issues they see in the changes and you can make those edits(this is general pull request back and forth). A maintainer will also comment if the pull request is approved/rejected and then add a tag for `Rejected`/`Approved`/`In Progress`.
    7. The maintainer will handle merging the pull request into the project and then closing the pull request. After it has been merged you can delete your branch in your forked repository.
---
5. QUESTION: This document is awesome! I want to donate money or something to show my support that it keeps getting updated! \
ANSWER: This document is meant to be 100% free with no strings attached, I WILL NOT accept any sort of donation's(unless its knowledge donations as pull requests to make the document better). I will keep updating it until I am physically no longer able to(we'll cross that bridge when we get to it). Anybody that contributes to this compendium should not be asking for donations to contribute, anybody found violating that rule will be immediately banned from contributing.
---
6. QUESTION: I have a suggestion for something to add to the document! What's the next steps? \
ANSWER: Please go to the "Issues" tab of this GitHub repository and make a new issue ticket for that. If you direct message any maintainers about it, we will just tell you the same thing or point you to this FAQ page.
---
7. QUESTION: How do I see what's planned for the document? \
ANSWER: So there's two parts to that;
  - There will be an issue ticket labeled `Planned xx.xx.xx Updates` and in there it will say is being worked on for the next update. You can see live progress on it where the maintainers will check off the privately filled out information(again the reason why is explained at the top of the FAQ), OR if somebody has made a Pull Request for it and it was merged into the public repository then its immediately available. The issue ticket will be closed and marked as done when all of the update's have been merged into the public repository.
  - For long term planned updates they are pretty much planned in my head but not being actively worked on or posted publicly, so that if somebody is working on one of that same chunk of knowledge as a public contribution then they don't have to assume its being worked on behind the scenes and can hopefully get it into the compendium earlier than if a maintainer did it themselves(updates/additions will be over time obviously).
---
8. QUESTION: What is the policy with the version numbers? \
ANSWER: The format is `Major.Minor.Patch` depending on the significance of the changes we will either update the `Major` or `Minor`. For each fix we increment the `Patch` but whenever the `Major` or `Minor` is incremented then we reset `Patch` to zero.
---
9. QUESTION: But what about section numbering within the document? \
ANSWER: Each section's number is `Major.Minor.Mini`, anything deeper in the nesting complicates the document and requires talking with a maintainer(this would be done through a Pull Request in the comments). 
---
10. QUESTION: I'm working on a contribution that I want to merge in but I saw a new update issue ticket was made that conflicts with my contribution, what do I do? \
ANSWER: First off, GREAT! Thank you for contributing, I really appreciate it! Secondly, make a draft pull request for the compendium so that it's progress is tracked(and possibly contribute to it if you want), then make a comment on the update ticket letting maintainers know that you're working on that contribution and include the draft pull request link. Afterwards maintainers will then tag your pull request with the Update tag to let you know that it is tracked.
---
11. QUESTION: What is the Compendium intended to cover when its pretty much completed? \
ANSWER: It's meant to explain both the runtime, rendering, and editor parts of UMG and Slate in the default Unreal Engine. Like I said in an earlier answer, its for the community and the community is welcome to help build it up! Now this doesn't mean it's not going to cover any plugins within the engine that you can take advantage of, but those would be as separate documents within the repository to make navigation easy and to help separate the non-essential stuff from the core information.  
--- 
12. QUESTION: What about in engine plugins like Common UI and Common Input and such? \
ANSWER: Since those aren't default Unreal Engine features and are at the end of the day, add ons(albeit very helpful addons) the compendium's main document will not cover it. BUT I do have plans to start covering it after I cover both the runtime and editor parts of UMG/Slate. They will also be separate documents within this same repository to make navigation of the knowledge bases easier.
---
13. QUESTION: Since you're not covering Common UI and Common Input immediately, can I start writing up documentation for it to add to this repository now? \
ANSWER: YES! You are 100% welcome to make a pull request and start adding Common UI and Common Input plugin documentation, I do require that you do them as separate .md files to make navigation easy for others and please try to follow the formatting structure that is in the base README file for the compendium which requires you to always point to where in the source code a system is(this helps avoid misinformation).

**[<span>&#11014;</span> Back to Top](#page-top)**
