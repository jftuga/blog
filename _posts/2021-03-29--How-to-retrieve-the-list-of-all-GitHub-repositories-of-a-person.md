---
layout: post
title: "How to retrieve the list of all GitHub repositories of a person?"
date: 2021-03-29 19:08:32 +0000
categories: github github-api
tags: github github-api
---

[## stackoverflow: How to retrieve the list of all GitHub repositories of a person?](https://stackoverflow.com/questions/8713596/how-to-retrieve-the-list-of-all-github-repositories-of-a-person)

Question Date: 2012-01-03 09:17:09

We are working on a project where we need to display all the projects of a person in his  repository on GitHub account.

Can anyone suggest, how can I display the names of all the git repositories of a particular person using his git-user name?

----

## [Answer](https://stackoverflow.com/a/66859755/452281)


Answer Date: 2021-03-29 14:37:48

Using the [official GitHub command-line tool](https://github.com/cli/cli):

```bash
    gh auth login

    gh api graphql --paginate -f query='
    query($endCursor: String) {
        viewer {
        repositories(first: 100, after: $endCursor) {
            nodes { nameWithOwner }
            pageInfo {
            hasNextPage
            endCursor
            }
        }
        }
    }
    ' | jq ".[] | .viewer | .repositories | .nodes | .[] | .nameWithOwner"
```

**Note**: this will include all of your public, private and other people's repos that are shared with you.

References:

* https://cli.github.com/manual/gh_api

