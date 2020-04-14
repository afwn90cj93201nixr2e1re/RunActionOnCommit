A GitHub app which allow trigger action on issue comment.

This app based on a probot solution.


## Usage
#### Create access configuration file (described at 'Access' section)


### Create a workflow
```yaml
name: 'Triggered from comment'
on:
    repository_dispatch:
        types: [BuildUniqueTitle]

jobs:
        one:
            runs-on: ubuntu-latest
            steps:
                - name: 'Dump GitHub context'
                  env:
                    GITHUB_CONTEXT: ${{ toJson(github) }}
                  run: echo "$GITHUB_CONTEXT"
 ```
#### Add a comment to an issue or a pull request

```bash
./sA 'BuildUniqueTitle' [args]
```
Supports all types of args single/double quoted.

### Payload inputs

| Name | Description |
| --- | --- |
| `client_payload.pComment` | Original comment body without `./sA` |
| `client_payload` | The full copy of original payload. |

### Where to find the id of a comment

How to find the id of a comment will depend a lot on the use case?
Everything can be touched via `${{ github.event.client_payload }}` object which contains full copy of `context.payload` and cleaned `comment.body` string.

### Access

For allowing access to certain groups of users use (bit-) sum of needed value:

[Available associations](https://developer.github.com/v4/enum/commentauthorassociation/)

| String | Int representation |
| --- | --- |
| COLLABORATOR | 1 |
| CONTRIBUTOR | 2 |
| FIRST_TIMER | 4 |
| FIRST_TIME_CONTRIBUTOR | 8 |
| MEMBER | 16 |
| NONE | 32 |
| OWNER | 64 |

For example for allowing running Actions only for OWNER(64) and COLLABORATOR's(1) you should set value to 65 = 1+64.

Append this value to file: `.github/.RunActionOnCommitAccess`, **attention** don't add any extra symbols to file.

Value cached for __120s__.

Actual link: https://github.com/afwn90cj93201nixr2e1re/RunActionOnCommit/blob/master/DEFAULT_BITS.md