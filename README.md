## renovatebot-discussion-28615

This repository demonstrates the following bug report submitted to renovate: https://github.com/renovatebot/renovate/discussions/28615

## Context
I'm developing a Python project with poetry as package manager and using renovate to update my dependencies. The project I'm developing should support all currently maintained Python versions. Some of my dependencies have dropped support for older versions of Python which are still activate (here 3.8). This means that I do not want to update certain dependencies beyond the latest version which still supports the older python version.

The default behaviour of renovate seems to be to suggest updates for these packages, even if it means dropping support for older versions of Python. Since I do not want to apply changes that will drop support for older Python versions, I want to configure renovate to avoid suggesting these changes. To achieve this, I have enabled the strict option for `constraintsFiltering`.

If I understand the docs correctly, renovate will retrieve the `python>=3.8` from my poetry config (I have verified this works) and use this as a constraint on all packages. However, when I run renovate with this configuration, it throws an error (Cannot destructure property 'epoch' of 'input' as it is null.") and skips any suggestions for the package due to the internal error. This seems to be a bug.

## Expected behaviour

I expect renovate to suggest an update for scikit-learn to the latest version which still matches the `python>=3.8` constraint of the `pyproject.toml`. This should be scikit-learn version 1.3.2 (https://github.com/scikit-learn/scikit-learn/blob/1.3.2/setup.py#L558). 


## Current behaviour

Renovate throws an error:

```json
DEBUG: Undefined release constraint
{
  "packageName": "scikit-learn"
  "versioning": "pep440"
  "constraint": null
}

WARN: AsyncResult: unhandled transform error
{
  "err": {
    "message": "Cannot destructure property 'epoch' of 'input' as it is null.",
    "stack": "TypeError: Cannot destructure property 'epoch' of 'input' as it is null.\n    at calculateKey (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/operator.js:95:11)\n    at compare (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/operator.js:53:22)\n    at ge (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/operator.js:38:10)\n    at contains (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/specifier.js:185:10)\n    at /usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/specifier.js:117:14\n    at Array.reduce (<anonymous>)\n    at /usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/specifier.js:113:19\n    at Array.filter (<anonymous>)\n    at pick (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/specifier.js:106:19)\n    at Object.satisfies [as matches] (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/specifier.js:123:20)\n    at /usr/local/renovate/lib/modules/datasource/common.ts:216:24\n    at filterMap (/usr/local/renovate/lib/util/filter-map.ts:13:17)\n    at applyConstraintsFiltering (/usr/local/renovate/lib/modules/datasource/common.ts:165:37)\n    at applyDatasourceFilters (/usr/local/renovate/lib/modules/datasource/index.ts:374:34)\n    at /usr/local/renovate/lib/workers/repository/process/lookup/index.ts:126:51\n    at /usr/local/renovate/lib/util/result.ts:790:28\n    at processTicksAndRejections (node:internal/process/task_queues:95:5)\n    at lookupUpdates (/usr/local/renovate/lib/workers/repository/process/lookup/index.ts:123:56)\n    at Function.wrap (/usr/local/renovate/lib/util/stats.ts:38:20)\n    at fetchDepUpdates (/usr/local/renovate/lib/workers/repository/process/fetch.ts:58:42)"
  }
}

ERROR: lookupUpdates error
{
  "currentValue": "1.2.2"
  "datasource": "pypi"
  "packageName": "scikit-learn"
  "followTag": null
  "lockedVersion": "1.2.2"
  "packageFile": "pyproject.toml"
  "pinDigests": false
  "rollbackPrs": false
  "updatePinnedDependencies": true
  "unconstrainedValue": false
  "err": {
    "message": "Cannot destructure property 'epoch' of 'input' as it is null.",
    "stack": "TypeError: Cannot destructure property 'epoch' of 'input' as it is null.\n    at calculateKey (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/operator.js:95:11)\n    at compare (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/operator.js:53:22)\n    at ge (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/operator.js:38:10)\n    at contains (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/specifier.js:185:10)\n    at /usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/specifier.js:117:14\n    at Array.reduce (<anonymous>)\n    at /usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/specifier.js:113:19\n    at Array.filter (<anonymous>)\n    at pick (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/specifier.js:106:19)\n    at Object.satisfies [as matches] (/usr/local/renovate/node_modules/.pnpm/@renovatebot+pep440@3.0.20/node_modules/@renovatebot/pep440/lib/specifier.js:123:20)\n    at /usr/local/renovate/lib/modules/datasource/common.ts:216:24\n    at filterMap (/usr/local/renovate/lib/util/filter-map.ts:13:17)\n    at applyConstraintsFiltering (/usr/local/renovate/lib/modules/datasource/common.ts:165:37)\n    at applyDatasourceFilters (/usr/local/renovate/lib/modules/datasource/index.ts:374:34)\n    at /usr/local/renovate/lib/workers/repository/process/lookup/index.ts:126:51\n    at /usr/local/renovate/lib/util/result.ts:790:28\n    at processTicksAndRejections (node:internal/process/task_queues:95:5)\n    at lookupUpdates (/usr/local/renovate/lib/workers/repository/process/lookup/index.ts:123:56)\n    at Function.wrap (/usr/local/renovate/lib/util/stats.ts:38:20)\n    at fetchDepUpdates (/usr/local/renovate/lib/workers/repository/process/fetch.ts:58:42)"
  }
}
```