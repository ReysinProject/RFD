---
authors: Mathias BIGAIGNON <m.bigaignon@xefi.fr>
state: prediscussion
pr_link:
labels: process
---
# Requests for Discussions

Thoroughly documenting technical decisions
and providing a proper space to discuss issues and proposal are two major concerns
for our teams, and something that we have historically struggled with.
Storing documents on the XEFI Wiki, in JIRA issues or in Teams documents doesn't really address these concerns, more often than not leading to documents being lost and technical discussions being forgotten.

This document establishes the Request for Discussion process as our answer to these issues.

## The subject matter
*Requests for Discussions* are written documents capturing any idea, proposal
or concept related to anything technical.

These are made in the original spirit of the IETF Request for Comments, established in 1969:

> The content of a note may be any thought, suggestion, etc. related to the software or other aspect of the network. Notes are encouraged to be timely rather than polished. Philosophical positions without examples or other specifics, specific suggestions or implementation techniques without introductory or background explication, and explicit questions without any attempted answers are all acceptable. The minimum length for a note is one sentence.

> These standards (or lack of them) are stated explicitly for two reasons. First, there is a tendency to view a written statement as ipso facto authoritative, and we hope to promote the exchange and discussion of considerably less than authoritative ideas. Second, there is a natural hesitancy to publish something unpolished, and we hope to ease this inhibition.

The process of RFCs is meant to provide a repository for established papers, a place to discuss ideas and concepts contained in these, and a history of these discussions and decisions.

### What RFDs can be used for

The long and short is that anything even remotely technical can be the subject of a RFD

Here are a few examples of topics that may become the topic of a RFC:
- New feature designs
- Architectural and design decision pertaining to a software system
- Changes to a public API
- Internal tooling ideas
- Updates to an internal process (CI/CD, deployment, monitoring, etc)

These are only some examples and meant to be broad; the common denominator is that RFD can discuss any technical concerns, whether directly related to our products or our internal processes.

RFD are, as stated in the above quote, encouraged to be created in a timely manner rather than in a polished one. It is expected that RFD authors follow up on their original proposals and not let them stagnate indefinitely. A RFD may be abandoned if the underlying idea is deemed not relevant anymore, but leaving in an undecided state should be avoided entirely.

### RFD Metadata

Every RFD starts with a header containing some metadata. The format is the following :
```
---
authors: Firstname LASTNAME <mail@mail.com>;
state: prediscussion
pr_link: <link the GitLab PR discussion>
labels: comma, separated, labels
---
```

A brief description of these elements:
1. `authors`: The semi-colon separated list of authors. Each author must be listed with their full name and email
2. `state`: Must be one of the state listed in the next section
3. `discussion`: For RFDs in the `discussion` state and above, this is a link to the relevant PR in GitLab
4. `labels`: A comma separated list of labels to make indexing and searching of RFD easier. The list of available labels is detailed in relevant section of this RFD.

The list of available states is:

0. `prediscussion`
1. `discussion`
2. `published`
3. `committed`
4. `abandoned`

The `prediscussion` state indicates a placeholder RFD. These are expected to be actively worked on by the author(s), so that they may be moved to the `discussion` state.

Documents under active discussions are in the `discussion` state. Discussions should be had in the appropriate PR. These may be fully formed documents, or may just be a description of the topic at hand calling for ideas and suggestions.

If and when a discussion reaches a consensus, the RFD should be moved to the `published` state. This state represent an idea, direction or implementation that
has been agreed upon and committed to.

Once implementation of an idea is complete, its associated RFD should be placed into the `committed` state. Further discussion on a committed idea should usually be made to raise an issue. If it represent an extension or severe divergence from the original idea, making an entirely new RFD may be more appropriate.

Finally, an idea deemed non-viable or deliberately not retained will be moved to the `abandoned` state.

## RFD Lifecycle

### Creating a RFD

To create a RFD, you must first reserve a RFD number. The next number available can be determined by looking at the result of this command:
```
> git branch -r
```
You may also look at the table in the `README.md` file in the main branch.
Once this has be determined, you may create your local branch with the appropriate number :
```
> git checkout -b 0042
```
You may then create a placeholder for your RFD by copying the template RFD found at the root of the repository, changing its name and number.
```
> cp 0000-template.md docs/0042-title.md
```
Fill in the appropriate metadata tags, then push your branch to the remote repository. At this point, your RFD should be in the `ideation` state.
```
> git add docs/0042-title.md
> git commit -m '0042: Adding placeholder for RFD <Title>'
> git push origin 0042
```
Once your branch is pushed, the table README.md file on the master branch will update automatically with your new RFD. Changing the metadata of that RFD may also update the appropriate entry in the table.

### Get writing!

You can now start writing! 

It is highly recommended that you push your changes quite often, so that your work gets properly saved in case on damaged local:

Your goal here is to get your document to a state where you are ready to receive external feedback. Once you are ready, change the RFD state and push your document:

```
> git commit -am '0042: Add RFD for <RFD Title>'
> git push
```
You may now open a MR to merge your RFD's branch into the main branch. Your MR should be titled `RFD{num}: {title}`. Once this is done, you will need to update the `pr_link` metadata in your document.

### Discussing a RFD

The author has full discretion regarding which comment and modification they want to accept.
When commenting on an RFD, do make sure that you provide constructive feedback. Ask yourself whether your comment is one you'd like reading if you were the author of that specific RFD (this does not mean that you can't *challenge* the author of a RFD on specific issues).

### Publishing the RFD

After sufficient feedback has been provided and you feel like the RFD has reached a state of consensus, its state can be moved to `published` and its branch merged. This timing is left to the author's discretion : about 5 business days is reasonable, but circumstances (lack of available expertise at this point in time, initial state of the document, complexity of the subject matter...) can change this timeline.
As a general rule, a RFD that has not received any feedback should never be merged!

### The aftermath

Once published, the original branch and its MR are retained. Discussion may continue one that MR, issues that merit more attention may be the subject of an issue or a new RFD.

Changes may be made after the fact on an `published` RFD to reflect evolution on the topic. One may do so simply by reopening a separate branch named after the rfd number and the focus of such changes (e.g. `0042_modification_to_a_section`). The process for discussions and reviews of these is essentially the same as for a normal RFD, making sure the author(s) have reviewed the changes beforehand.

RFD that have been actually committed to and fully implemented should be marked as `committed`. This is essentially not different from a `published` RFD, except further discussions and changes are expected to be less frequent.

## Tooling

### Labels

Labels are metadata pieces used to describe some high-level category that the RFD falls into.

- `process` : Related to any kind of process
- `backend` : Self explanatory
- `frontend` : Self explanatory
- `infra` : Related to infrastructure concerns
- `arch` : Related to systems architecture
- `tooling` : Related to dev tools
- `metrics` : Related to analytics, metrics or monitoring
- `security` : All things related to security concerns
- `feature` : Anything feature related (spec, design, implementation)

## References

- [IETF RFCs](https://www.ietf.org/process/rfcs/)
- [Oxide RFDs](https://rfd.shared.oxide.computer/rfd/0001)
- [Joyent RFDs](https://github.com/TritonDataCenter/rfd)
