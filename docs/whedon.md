Interacting with Whedon
========================

Whedon or `@whedon` on GitHub, is our editorial bot that interacts with authors, reviewers, and editors on JOSS reviews.

`@whedon` can do a bunch of different things. If you want to ask `@whedon` what it can do, simply type the following in a JOSS `review` or `pre-review` issue:

```text
# List all of Whedon's capabilities
@whedon commands

# Assign a GitHub user as the sole reviewer of this submission
@whedon assign @username as reviewer

# Add a GitHub user to the reviewers of this submission
@whedon add @username as reviewer

# Re-invite a reviewer (if they can't update checklists)
@whedon re-invite @username as reviewer

# Remove a GitHub user from the reviewers of this submission
@whedon remove @username as reviewer

# List of editor GitHub usernames
@whedon list editors

# List of reviewers together with programming language preferences and domain expertise
@whedon list reviewers

# Change editorial assignment
@whedon assign @username as editor

# Set the software archive DOI at the top of the issue e.g.
@whedon set 10.0000/zenodo.00000 as archive

# Set the software version at the top of the issue e.g.
@whedon set v1.0.1 as version

# Open the review issue
@whedon start review

EDITORIAL TASKS

# All commands can be run on a non-default branch, to do this pass a custom 
# branch name by following the command with `from branch custom-branch-name`.
# For example:

# Compile the model
@whedon generate pdf

# Compile the model from alternative branch
@whedon generate pdf from branch custom-branch-name

# Remind an author or reviewer to return to a review after a
# certain period of time (supported units days and weeks)
@whedon remind @reviewer in 2 weeks

# Ask Whedon to do a  dry run of accepting the model and depositing with Crossref
@whedon recommend-accept

# Ask Whedon to check the references for missing DOIs
@whedon check references

# Ask Whedon to check repository statistics for the submitted software, for license, and
# for Statement of Need section in model
@whedon check repository

EiC TASKS

# Flag submission for editoral review, due to size or question about being research software
@whedon query scope

# Invite an editor to edit a submission (sending them an email)
@whedon invite @editor as editor

# Reject a model
@whedon reject

# Withdraw a model
@whedon withdraw

# Ask Whedon to actually accept the model and deposit with Crossref
# (supports custom branches too)
@whedon accept deposit=true
```

## Author commands

A subset of the Whedon commands are available to authors (and reviewers):

### Compiling models

When a `pre-review` or `review` issue is opened, `@whedon` will try to compile the JOSS model by looking for a `model.md` file in the repository specified when the model was submitted.

If it can't find the `model.md` file it will say as much in the review issue. If it can't compile the model (i.e. there's some kind of Pandoc error), it will try and report that error back in the thread too.

```eval_rst
.. note:: If you want to see what command ``@whedon`` is running when compiling the JOSS model, take a look at the code `here <https://github.com/openjournals/whedon/blob/195e6d124d0fbd5346b87659e695325df9a18334/lib/whedon/processor.rb#L109-L132>`_.
```

Anyone can ask `@whedon` to compile the model again (e.g. after a change has been made). To do this simply comment on the review thread as follows:

```text
@whedon generate pdf
```

#### Compiling models from a non-default branch

By default, Whedon will look for models in the default git branch. If you want to compile a model from a non-default branch, this can be done as follows:

```text
@whedon generate pdf from branch custom-branch-name
```

### Finding reviewers

Sometimes submitting authors suggest people the think might be appropriate to review their submission. If you want the link to the current list of JOSS reviewers, type the following in the review thread:

```text
@whedon list reviewers
```

## Editorial commands

Most of `@whedon`'s functionality can only be used by the journal editors.

### Assigning an editor

Editors can either assign themselves or other editors as the editor of a submission as follows:

```text
@whedon assign @editorname as editor
```

### Inviting an editor

Whedon can be used by EiCs to send email invites to registered editors as follows:

```text
@whedon invite @editorname as editor
```

This will send an automated email to the editor with a link to the GitHub `pre-review` issue.

### Adding and removing reviewers

Reviewers should be assigned by using the following commands:

```text
# Assign a GitHub user as the sole reviewer of this submission
@whedon assign @username as reviewer

# Add a GitHub user to the reviewers of this submission
@whedon add @username as reviewer

# Remove a GitHub user from the reviewers of this submission
@whedon remove @username as reviewer
```

```eval_rst
.. note:: The ``assign`` command clobbers all reviewer assignments. If you want to add an additional reviewer use the ``add`` command.
```

### Starting the review

Once the reviewer(s) and editor have been assigned in the `pre-review` issue, the editor starts the review with:

```text
@whedon start review
```

```eval_rst
.. important:: If a reviewer recants their commitment or is unresponsive, editors can remove them with the command ``@whedon remove @username as reviewer``. You can also add new reviewers in the ``REVIEW`` issue, but in this case, you need to manually add a review checklist for them by editing the issue body.
```

### Reminding reviewers and authors

Whedon can reminders authors and reviewers after a specified amount of time to return to the review issue. Reminders can only be set by editors, and only for REVIEW issues. For example:

```text
# Remind the reviewer in two weeks to return to the review
@whedon remind @reviewer in two weeks
```

```text
# Remind the reviewer in five days to return to the review
@whedon remind @reviewer in five days
```

```text
# Remind the author in two weeks to return to the review
@whedon remind @author in two weeks
```

```eval_rst
.. note:: Most units of times are understood by Whedon e.g. `hour/hours/day/days/week/weeks`.
```

```eval_rst
.. important:: For reviewers, the reminder will only be triggered if the reviewer's review is outstanding (i.e. outstanding checkboxes).
```

### Setting the software archive

When a submission is accepted, we ask that the authors create an archive (on [Zenodo](https://zenodo.org/), [fig**share**](https://figshare.com/), or other) and post the archive DOI in the `REVIEW` issue. The editor should add the `accepted` label on the issue and ask `@whedon` to add the archive to the issue as follows:

```text
@whedon set 10.0000/zenodo.00000 as archive
```

### Changing the software version

Sometimes the version of the software changes as a consequence of the review process. To update the version of the software do the following:

```text
@whedon set v1.0.1 as version
```

## Accepting a model (dry run)

Whedon can accept a model from the review issue. This includes generating the final model PDF, Crossref metedata, and depositing this metadata with the Crossref API.

JOSS topic editors can ask for the final proofs to be created by Whedon with the following command:

```text
@whedon recommend-accept
```

On issuing this command, Whedon will also check the references of the model for any missing DOIs. This command can be triggered separately:

### Check references

```text
@whedon check references
```

```eval_rst
.. note:: Whedon can verify that DOIs resolve, but cannot verify that the DOI associated with a model is actually correct. In addition, DOI suggestions from Whedon are just that - i.e. they may not be correct.
```

## Accepting a model (for real)

If everything looks good with the draft proofs from the `@whedon accept` command, JOSS editors-in-chief can take the additional step of actually accepting the JOSS model with the following command:

```text
@whedon accept deposit=true
```

```eval_rst
.. note:: This command is only available to the JOSS editor-in-chief, or associate editors-in-chief.
```
