# Gitlab flow implement on multi-environment
- Add standardized pattern / practices on Git usage

## Official doc
- [https://docs.gitlab.com/ee/topics/gitlab_flow.html](https://docs.gitlab.com/ee/topics/gitlab_flow.html)

## Current Situation
We originally have created 4 branches including master in our project, from DEV, UAT, Staging and Production. We develop new feature like the trunk-based approach on the dev branches. Then we merge our branch from dev to uat, uat to staging and staging to production.

## Problems
Things work fine if we are deploying all features from dev to production. But after a period of time, we find that we need to hold some features in dev, some hold at uat. To do this we have to cherry pick the features to uat and staging each time when there are some features held which we cannot directly merge the branch forward.

## Approach

### Version branch
As DEV, UAT and STAGE usually have varied features deployed, their version is separated and not related, so they should NOT merge to each other.
Except for merging STAGE to master, stage should be the pre-production version for final testing that version before deploy to production.
No direct commit to version branch, only through merge request, hotfix should be handled like normal bug fix below

### New feature/Bug fix
Always create a feature branch FROM STAGING. 
the feature branch should be deleted once its merge back to staging.
if staging updated version, should update the feature branch by merge staging branch to feature branch
So all exist feature branches is "in-progress" features that not yet plan to deploy in stage and then prod

### Deploy feature to version branch
create a release branch from the version branch, then merge the feature branch/branches into the release branch, resolve the merge conflict,
update changelog.md and version number, push to remote and create merge request to merge the releasing branch to version branch, 
should merge without fast forward or squash so no commit infomation is lost.
Add version tag after merge

### Version number, changelog
mark changes in feature branch as [unreleased ], add the unreleased changelog part to each version branch changelog separately when deploying feature.
dev, uat, stage is three separated version not related to each other, so their version/changelog should be handled separately for each version branch.

