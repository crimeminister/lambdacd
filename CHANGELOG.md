# Changelog

This changelog contains a loose collection of changes in every release. I will also try and document all breaking changes to the API.

The format is based on [Keep a Changelog](http://keepachangelog.com/) and this project adheres to a "shifted" version of semantic versioning while the major version remains at 0: Minor version changes indicate breaking changes, patch version changes should not contain breaking changes.

## 0.14.5 

### Security

* Updated a couple of dependencies that had known vulnerabilities. None of these are known to be exploitable in LambdaCD, however upgrading can make sense as a precaution

## 0.14.4 

### Security

* Updated a couple of dependencies that had known vulnerabilities. None of these are known to be exploitable in LambdaCD, however upgrading can make sense as a precaution

## 0.14.3

### Fixed

* Made loading pipeline history a bit more robust in an attempt to improve the fix for #192

## 0.14.2

### Fixed

* Fixed bug that prevented LambdaCD from starting if home-dir contained build history directories without a build number in their name (#192)

## 0.14.1

Thanks to @Atsman for cleaning up and improving the frontend code in this release!

### Changed

This release contains update of frontend libs and related tools such as: reagent, reframe, figwheel etc.

### Fixed

* Reduced number of requests from frontend. In case of slow network or in case of server issues, frontend doesn't send req before it got response or timeout error for previous ones. [UI request rate #187](https://github.com/flosell/lambdacd/issues/187)


## 0.14.0

**House-Keeping Release**. 

This release contains no new functionality but only cleans up the dependencies, codebase and removes deprecated interfaces. 

These are **breaking changes** so if you are still using deprecated features or rely on some very specific behavior, take care when upgrading.

### Added

* Support for Clojure 1.9 (#178)

### Changed

* LambdaCD now depends on Clojure 1.8 by default. Clojure 1.7 is no longer officially supported but might still work.  

### Removed

* Removed `lambdacd.internal.pipeline-state/PipelineStateComponent` (deprecated since 0.11.0). Use the functions in `lambdacd.state.core` access state or the protocols in `lambdacd.state.protocols` to implement custom persistence functionality.
* Removed `lambdacd.util` (deprecated since 0.12.1)
* Removed `lambdacd.ui.ui-server` (deprecated since 0.13.1)
* Removed `lambdacd.execution` (deprecated since 0.13.0)
* Removed `lambdacd.steps.result` (deprecated since 0.13.1)
* Removed `lambdacd.steps.status` (deprecated since 0.13.1)
* Removed `lambdacd.steps.support` (deprecated since 0.13.1)
* Removed functions in  `lambdacd.core` (deprecated since 0.9.5)
* Made helper-functions in `lambdacd.ui.ui-page` private (use was deprecated since 0.13.1)
* Made helper-functions in `lambdacd.presentation.pipeline-structure` private (use was deprecated since 0.13.1)
* Made helper-functions in `lambdacd.runners` private (use was deprecated since 0.13.1)
* Made helper-functions in `lambdacd.presentation.pipeline-structure` private (use was deprecated since 0.13.1)
* Made helper-functions in `lambdacd.steps.control-flow` private (use was deprecated since 0.13.1)
* Made helper-functions in `lambdacd.steps.shell` private (use was deprecated since 0.13.1)
* Made helper-functions in `lambdacd.presentation.pipeline-state` private (use was deprecated since 0.13.1)
* Removed helper functions in `lambdacd.presentation.unified` private (were deprecated since 0.13.1)
* Remove behavior that allowed calling `lambdacd.presentation.pipeline-state/history-for` with state instead of ctx (deprecated since 0.11.0)

## 0.13.5

### Fixed

* Output of steps that throw an exception gets lost (#174)

## 0.13.4

### Fixed

* `nil`-values in the pipeline-structure (e.g. from conditionals when creating it) led to step-information being in the wrong place for the UI (#172)

## 0.13.3

### Fixed

* Replaced dependeny `org.ow2.proactive/process-tree-killer` that was only available in an insecure maven repo with `com.jezhumble/javasysmon` so that LambdaCD Projects can be built with Leiningen 2.8.0 and later (fixes #171)

## 0.13.2

### Fixed

* `java.lang.ClassNotFoundException: clojure.test` in `lambdacd.presentation.pipeline-structure` under some circumstances (#169)

## 0.13.1

### Added

* Proper API Documentation: http://www.lambda.cd/api-docs/0.13.1/

### Fixed
 
* Marking builds as dead if the datastore sees them as running but they are not (e.g. because LambdaCD was not cleanly shut down previously). 
    This introduces the new `:status :dead` (#134)
* `junction` did not behave like a sequential step, i.e. it did not pass the results of the condition to the arguments of the branch steps (#162)
* made `lambdacd.stepresults.merge/merge-step-results` default behavior consistent with other step-result merging strategies: should concatenate colliding lists

### Deprecated

* Built-in Git-Support (`lambdacd.steps.git`) should be considered deprecated now and will be removed in the future. Use the more modern [`lambdacd-git` library](https://github.com/flosell/lambdacd-git) instead.
* A couple of functions were only public by accident and should not be considered part of the public API. They will be moved or become private in the future: 
  * `lambdacd.presentation.pipeline-state/not-retriggered?`
  * `lambdacd.presentation.pipeline-structure/pad`
  * `lambdacd.presentation.pipeline-structure/step-display-representation-internal`
  * `lambdacd.runners/should-trigger-next-build?`
  * `lambdacd.steps.control-flow/synchronize-atoms`
  * `lambdacd.steps.shell/kill`
  * `lambdacd.steps.status/choose-last-or-not-success`
  * `lambdacd.steps.support/{replace-args-and-ctx,to-fn,to-fn-with-args,unify-results}`
  * `lambdacd.ui.ui-page/{css-includes,js-includes,favicon,app-placeholder,title,header,ui-config,ui-page}`
  
* `lambdacd.presentation.unified/unified-presentation` is unused and therefore deprecated and will be removed in the future. Use `pipeline-structure-with-step-results` instead.
* `lambdacd.steps.support/merge-globals` is no longer necessary, normal step result merging (`merge-step-results`,`merge-two-step-results`) should already conserve and merge globals just fine
* A couple of functions were moved to make the package structure more clear. The existing functions still exist but are now deprecated and will be removed in the future.
  * `lambdacd.steps.result/flatten-step-result-outputs` moved to `lambdacd.stepresults.flatten`
  * `lambdacd.steps.result/{merge-step-results,merge-two-step-results}` moved to `lambdacd.stepresults.merge`
  * `lambdacd.steps.result/*-resolver` moved to `lambdacd.stepresults.merge-resolvers`
  * `lambdacd.steps.status/successful-when-*` moved to `lambdacd.stepstatus.unify`
  * `lambdacd.steps.status/is-active?` moved to `lambdacd.stepstatus.predicates`
  * `lambdacd.steps.support/assoc-build-metadata!` moved to `lambdacd.stepsupport.metadata`
  * `lambdacd.steps.support/unify-only-status` moved to `lambdacd.stepstatus.unify`
  * `lambdacd.steps.support/{capture-output,printed-output,print-to-output,set-output,new-printer}` moved to `lambdacd.stepsupport.output`
  * `lambdacd.steps.support/{killed?,if-not-killed}` moved to `lambdacd.stepsupport.killable`
  * `lambdacd.steps.support/{chaining,chain-steps,always-chaining,always-chain-steps,injected-args,injected-ctx,last-step-status-wins}` moved to `lambdacd.stepsupport.chaining`
  * `lambdacd.ui.ui-server/ui-for` moved to `lambdacd.ui.core`
  
  

## 0.13.0

### Added

* Added support for build-level metadata (#138). See [Build Metadata](https://github.com/flosell/lambdacd/wiki/Build%20Metadata) for details
* UI support for some kinds of metadata: 
  * `:human-readable-build-label`
* Added events `:pipeline-started` and `:pipeline-finished` (#155)
* Added function to simplify handling of nested step-results (e.g. the information received from `:pipeline-finished` events (#155, #154): 
* `lambdacd.steps.result/flatten-step-result-outputs`
* Added functions to simplify getting information about a specific step (#154):
* `lambdacd.presentation.pipeline-structure/flatten-pipeline-representation`
* `lambdacd.presentation.pipeline-structure/step-display-representation-by-step-id`

### Fixed

* Catch Exception instead of Throwable in build steps to avoid catching Errors which cannot be handled (#148), thanks @hgsy!

### Changed
* Changes in internal API:
 `lambdacd.internal.execution` was refactored into several independent namespaces, functions were moved around, replaced or made private. 
    You shouldn't have dependencies on those unless you are doing something really crazy or advanced. If you did, please consider using functions in public namespaces (i.e. that don't have `internal` in their name).
    If you have dependencies on functions that have no public equivalent, please open an issue to get this fixed. 

### Deprecated

* `lambdacd.execution` was deprecated in favor of `lambdacd.execution.core`

### Removed

* Removed `:unify-status-fn` parameter in `execute-steps` (deprecated since 0.9.4). Use `:unify-results-fn` instead. `lambdacd.steps.support/unify-only-status` can help with migrating unify-status-fns.


## 0.12.1

New years cleanup and bug fix release.

* Bug fixes: 
  * **0.12.0 was released without proper CSS, this release is fixing this.** 
* API Changes:
  * `lambdacd.util` was cleaned up or moved to separate, internal namespaces as most of this functionality was never intended to be part of the public namespace. If you depend on utility functions and feel they should be part of LambdaCDs public API, please open an issue. Specifically, the following functions are now deprecated
    * `lambdacd.util/write-as-json`
    * `lambdacd.util/ok`
    * `lambdacd.util/bash`
    * `lambdacd.util/range-from`
    * `lambdacd.util/map-if`
    * `lambdacd.util/no-file-attributes`
    * `lambdacd.util/temp-prefix`
    * `lambdacd.util/create-temp-dir`
    * `lambdacd.util/create-temp-file`
    * `lambdacd.util/with-temp`
    * `lambdacd.util/json`
    * `lambdacd.util/to-json`
    * `lambdacd.util/put-if-not-present`
    * `lambdacd.util/parse-int`
    * `lambdacd.util/contains-value?`
    * `lambdacd.util/buffered`
    * `lambdacd.util/fill`
    * `lambdacd.util/merge-with-k-v`

## 0.12.0

**This release was released without proper CSS, use 0.12.1**

* Bug fixes: 
  * Fixed retriggering: Retriggering did not work if the new pipeline state was used as it did not save the pipeline structure for the retriggered build (#146). 
  * Fixed a race condition in event-bus unsubscribe that had potential to deadlock the system in rare circumstances (#145).
  * Rewrote event-bus to prevent deadlocks under heavy load (#144). As this new event-bus is not battle-tested yet, it is not active by default. Use he config setting `:use-new-event-bus true` to activate it. This will become the default in upcoming releases.  
* Breaking Changes: 
  * Removed `lambdacd.event-bus/publish` (deprecated since 0.9.1), use `lambdacd.event-bus/publish!!` instead (or `lambdacd.event-bus/publish!` when being called from a go-block)  

## 0.11.0

* Improvements: 
  * Keeps a history of pipeline structure if persistence component supports it (#131, #6); Implemented for default persistence
  * Improved performance and resource consumption by compressing and throttling step-result update events (#140). 
    Can be configured with the configuration parameter `:step-updates-per-sec`.
  * Introduced event `:step-result-update-consumed` to indicate that a step update was consumed and is available in the pipeline state #136
* Bug fixes:
  * Fix deadlock occurring when steps write a lot of step-results in quick succession and step results are inherited by their parents (as in chaining) (#135, #140)
* API changes:
  * New state handling (#131): 
    * Protocols in `lambdacd.state.protocols` replace `lambdacd.internal.pipeline-state/PipelineStateComponent` which is now deprecated. Custom persistence-mechanisms need to migrate.
    * Added facade `lambdacd.state.core` for all state-related functionality. Access directly to `PipelineStateComponent` is now deprecated.
    * `lambdacd.presentation.pipeline-state/history-for` should now be called with ctx; Calling it with a build-state (the result of `lambdacd.internal.pipeline-state/get-all`) still works but is now deprecated.
    * `lambdacd.presentation.unified/unified-presentation` is now deprecated, use `lambdacd.presentation.unified/pipeline-structure-with-step-results` instead
  * The current pipeline-definition can now be accessed as `:pipeline-def` in ctx
* Breaking Changes:
  * Moved pipeline-state-updater from `lambdacd.internal.pipeline-state` to `lambdacd.state.internal.pipeline-state-updater` and refactored interface. As this is an internal namespace, it should not affect users unless they customized LambdaCDs startup procedure to a large degree.
  * The fix for #135 changes the behavior of step result inheritance by introducing a sliding window that compresses several step result update events into one: Steps inheriting their childens results via the `:unify-status-fn` or `:unify-results-fn` (e.g. chaining steps) might not pass on intermediate update events; the ultimately resulting unified step result will remain the same.
  * Removed `nil`-check from `DefaultPipelineState/{update,consume-step-result-update}`: This was meant as a convenience for internal tests that set up incomplete components. Tests have since been fixed so this is no longer necessary. If you are impacted by this issue, make sure you create `DefaultPipelineState` with `new-default-pipeline-state`

## 0.10.0

* Bug Fixes: 
  * Fixed critical issue that prevented release 0.9.5 from even starting #133
* Breaking Changes:
  * Removed backwards compatibility for versions older than 0.8.0 that write their history in JSON instead of EDN. If you want to keep your history, upgrade to 0.9.4 first and then upgrade to 0.10.0.
  * Removed namespace `lambdacd.internal.step-id` (was deprecated since 0.7.0), use `lambdacd.step-id` instead

## 0.9.5

**This release is broken (#133), do not use** 

* Improvements: 
  * Allow truncating build history by setting `:max-builds` in config (#132). Defaults to `Integer/MAX_VALUE` so this should be a non-breaking change
  * Added `lambdacd.execution/run` to the public namespace. If you were using `lambdacd.internal.execution/run` until now, migrate to make sure you are using the official public namespace as internal interfaces can change without notice (#128)
* API changes: 
  * Moved public API to interact with execution engine from `lambdacd.core` into separate namespace `lambdacd.execution`. `lambdacd.core/{retrigger,kill-step,execute-steps,execute-step}` are now deprecated and will be removed in subsequent releases. Use the equivalent functions in `lambdacd.execution` instead

## 0.9.4

* Improvements: 
  * `lambdacd.steps.support/{chain,always-chain,chaining,always-chaining}` now return outputs of individual chained steps (#122)
  * Add `lambdacd.steps.support/last-step-status-wins` to coerce a step result into having the status of the last output
    to make an always-chained step successful even though it had a failing step in it (#122)
  * Add `:unify-results-fn` to unify the whole step-result, not just the step status from children in `core/execute-steps`
* Bug fixes: 
  * Refactored merging of step results and resolved overly broad merging behavior (see breaking changes)
  * Chaining no longer loses intermediate results (fixes #120)
* API changes:
  * The `:unify-status-fn` parameter in `core/execute-steps` is now deprecated and will be removed in subsequent releases. 
    Use `:unify-results-fn` instead. 
* Breaking changes: 
  * Changed behavior of step-merging in some edge-cases where it was merging with special behavior in cases that were not necessary.
    This change should not affect normal pipeline behavior unless they rely on this very edge-case. 

## 0.9.3

* Improvements:
  * UI: Add kill button to waiting steps (#115)
  * UI: Improve visualization of steps that are in the progress of being killed
* Bug fixes:
  * Fix status aggregation that led to parent steps showing a wrong status in some situations with deeply nested
    container steps (#116)
  * Fix bug that caused a whole `either` step to be killed when one of its children was killed (#118)
  * UI: Fix bug that caused no indication that child steps were in the progress of getting killed by their parents (#117)

## 0.9.2

* Improvements:
  * UI: Clear pipeline-state when switching build to get rid of perceived "lag" while waiting for new state to load
  * UI: Improve look&feel of loading behavior
  * UI: kill and retrigger-buttons away from the expand/collapse button to prevent users from accidentally clicking
    the wrong thing (#69)
* Bug fixes: 
  * Fixed bug that broke `with-workspace` when the workspace contained circular symlinks (#112)

## 0.9.1

* Bug fixes:
  * Fixed bug that led to stuck pipelines in scenarios where a lot of pipelines live in the same project/process (#110)
  * Fixed bug that prevented nil values to appear in a pipeline and made it hard to implement optional steps in the pipeline structure (#111)
* API Changes:
  * `lambdacd.event-bus/publish` is now deprecated in favor of `lambdacd.event-bus/publish!!` and `lambdacd.event-bus/publish!`
     (to be able to properly publish from within a go-block)

## 0.9.0

* Improvements:
  * Added `lambdacd.steps.support/always-chain-steps` and `lambdacd.steps.support/always-chaining` to support chaining 
    build-steps without stopping on error, to allow post-processing steps, e.g. test-report processors. (#108)
    Check the [wiki](https://github.com/flosell/lambdacd/wiki/Step-Support#always-chain-steps-args-ctx--steps) for details
* Breaking Changes: 
  * Removed deprecated support for calling `lambdacd.steps.support/chain-steps` with a vector of steps, use varargs instead
  * Removed deprecated function `lambdacd.steps.support/chain`, use `chaining` instead

## 0.8.0
* Improvements: 
  * UI: Trigger symbol is now visible before a manual trigger is reached (#97)
  * UI: Console output now supports basic ANSI escape sequences (#91)
  * UI: Default expand behavior now configurable (#99)
    For details see https://github.com/flosell/lambdacd/wiki/Configuration
  * Started to add features for clean shutdown (#103)
    * Made pipeline runners stoppable (#78)
    * Made persistence mechanism stoppable
* Bug fixes:
  * UI did not display the second detail if it had the same label as the first (#98)
  * Fixed bugs in persistence that led to some data becoming corrupted between restarts (#101)
  * Fixes rendering of complicated map-keys (like "refs/heads/master") in complete step result (#100)
* Breaking Changes:
  * Changed the default persistence format from JSON to EDN to fix #101. This change should be backwards and forwards
    compatible (i.e. you keep your history when upgrading to 0.8.0 and when downgrading to an earlier version). However,
    if your history is critical, consider backing up your LambdaCD home-dir just in case.
  * Removed deprecated function `lambdacd.presentation.pipeline_state/most-recent-build-number-in`
  * LambdaCD no longer depends on Logback as a logging implementation and gives you more freedom to choose a logging
    implementation. If you see the following message you need to add a dependency to a library that's compatible with SLF4J:

    ```
    SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
    SLF4J: Defaulting to no-operation (NOP) logger implementation
    SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
    ```

    The default dependencies were:

    ```clojure
    [ch.qos.logback/logback-core "1.0.13"]
    [ch.qos.logback/logback-classic "1.0.13"]
    ```
  * LambdaCD no longer depends on ring-server and leaves the decision on how you serve the UI to you. If you see the
    following error, you were probably using ring-server and need to add it to your dependencies:
    ```
    Exception in thread "main" java.io.FileNotFoundException: Could not locate ring/server/standalone__init.class or ring/server/standalone.clj on classpath.
    ```

    The default dependency was:

    ```clojure
    [ring-server "0.3.1"]
    ```

## 0.7.1
* Improvements: 
  * UI: Adds ability to display preformatted text in step result details (#89)
  * UI: Honors ASCII escape sequences like `\r` in console output (#88)
* Bug fixes:
  * UI: Unicode Characters displayed as ??? (#92)
  * Chaining macro did not inject `args` and `ctx` if they were referred to via the namespace (#93)
  * Fixed merging of step results when both values in the maps to be merged are seqs (#95) (thanks @thilo11)

## 0.7.0
* Improvements: 
  * Adds public functions to simplify building custom build state aggregations (#84)
    * `lambdacd.presentation.pipeline-state/overall-build-status`
    * `lambdacd.presentation.pipeline-state/latest-most-recent-update` 
    * `lambdacd.presentation.pipeline-state/earliest-first-update` 
    * `lambdacd.presentation.pipeline-state/build-duration`
    * Renames namespace `lambdacd.internal.step-id` to `lambdacd.step-id` to officially make it public. 
      In case someone was using the internal namespace, it is still there but is considered DEPRECATED and will be removed in subsequent releases. 
  * UI: Link LambdaCD header to "/" to link back to overview in cases with multiple pipelines (#82)
  * UI: Refactored the server side UI code to make it simpler to customize the UI (#83)
* Bug fixes:
  * `with-workspace` now creates temporary directories in the home-dir (#79)
* API Changes:
  * `lambdacd.presentation.pipeline-state/most-recent-build-number-in` seems to be unused and now considered DEPRECATED.
    Will be removed in subsequent releases.
  * Removed `(ui-server/ui-for pipeline-def pipeline-state ctx)` (deprecated since 0.4.3). Use `(ui-server/ui-for pipeline)` instead.

## 0.6.1
* Improvements:
  * Adds `with-workspace` container step that allows users to run operations in the context of a clean workspace on disk (#72)
  * UI: expand active steps per default

## 0.6.0
* Improvements: 
  * UI: Adding feature to collapse/expand all, only active or only failed steps (#59)
* Bug fixes:
  * `either` no longer aggregates to failure if only some children failed (#67)
  * `in-parallel` no longer aggregates to failure while other children are still running (#68)
* Breaking Changes: 
  * `successful-when-one-successful` status aggregation only fails if all statuses are `:failure` (#67)
  * `successful-when-all-successful-sequential` status aggregation doesn't fail while other steps are still running (#68)

## 0.5.7
* Improvements:
  * UI: Displaying duration of each build step (#34)
  * Prevent retriggering of steps that have dependencies to previous steps by adding `:depends-on-previous-steps true` to metadata:
  
    ```clojure
    (defn ^{:depends-on-previous-steps true} publish-artifact [{cwd :cwd} ctx]
      (shell/bash ctx cwd
                  "./publish.sh"))
    ```
  
    This can be useful if several steps work on a workspace created by a nested step (such as `with-git`) and rely on the products of previous steps. 
    See #36 for details. 
  * Improved calculation of build duration for retriggered pipelines (#30)
  * UI: collapsing child steps by default (#59)
  * Add feature to alias build steps in UI:

    ```clojure
    ; this displays "trigger" instead of "either" in UI
    (alias "trigger"
      (either
        wait-for-manual-trigger
        wait-for-repo))
    ```
  
  * UI: Added feedback after killing a step
  * Killing a `shell/bash`-step now kills the whole process tree spawned by it. This helps in cases where the step spawns
    longer-running processes and bash doesn't pass on the TERM signal to its children)
  * Fixed UI bug where pipeline was no longer visible for long step output

## 0.5.6

* Improvements: 
  * Added `lambdacd.steps.support/capture-output` to simplify working with stdout in build-steps (#60)
  * Added `lambdacd.steps.support/chaining`, a more flexible and powerful variant of the existing `chain` macro. (#39)
    It supports injecting `args` and `ctx` at random places and, together with capture-output, also allows for 
    easy debugging:
     
    ```clojure
    (chaining {} {}
      (some-step injected-args injected-ctx)
      ; prints the foo value that's returned by some-step and is injected into some-other-step
      (print "foo-value:" (:foo injected-args)) 
      (some-other-step injected-args injected-ctx))
    ```
    
    This change also DEPRECATES `lambdacd.steps.support/chain` which will be removed in subsequent releases. 
* Bug fixes: 
  * `:global` values can now be overwritten by later stages in the pipeline (#61)
    
## 0.5.5
* Improvements: 
  * UI: redesigned build history:
    * added information on when the build was triggered (#52)
    * added information when a build was retriggered

## 0.5.4
* Improvements:
  * UI: Display Pipeline-Name in `title` tag of UI if configured.
  * UI: Prettier favicon (thanks @alphaone for this)
  * UI: Jump to most recent build if none given 
* Bug fixes:
  * Fix bug that led to wrong `first-updated-at` timestamps when retriggering children of container-steps (#56, flosell/lambdacd-cctray#3)
  * UI: fix bug that led to console output being refreshed all the time, making selecting text hard (#53)
  * UI: fix bug that broke UI in Firefox 41 (#57)
* Breaking Changes:
  * LambdaCD now requires Clojure 1.7

## 0.5.3
* Improvements:
  * Adding `with-git-branch` that always checks out the latest commit on a particular branch (as opposed to `with-git`
    which checks out a revision given in `args` or the `master` branch in case nothing is given) (#46) (thanks @exload
    for this)
  * UI: Further improvements to scrolling behavior with long pipelines
* Bug fixes: 
  * Fix a bug that lead to the fact that container steps that were the only children of another container step would not
    be shown in the UI (#47)

## 0.5.2
* Improvements:
  * UI: display the name of the pipeline if config contains a value for `:name`
  * UI: make sure build history doesn't shrink when pipeline content is very long
  * UI: make sure long pipeline output doesn't spill over the visible width
  * UI: remove alert after clicking the manual trigger

## 0.5.1
* Improvements:
  * Made `:display-type :container` the default for container steps, no longer throwing incomprehensible exceptions
    when display-type declarations are forgotten (#43)
  * Now supporting steps with parameters in the pipeline:
   
    ```clojure
    (defn mk-pipeline-def [{repo-uri :repo-uri test-command :test-command}]
      `(
         wait-for-manual-trigger
         (with-repo ~repo-uri
                    (run-tests ~test-command)
                    publish)))
    ```
  * Polished the UI

## 0.5.0

* Improvements:
  * UI: add support for :details field in step-results to display details about a build step result and link to more 
    detailed information (e.g. test reports) (#37)
  * UI: packaged icon font instead of relying on external CDN
  * UI: added vendor prefixes in CSS to improve browser support (esp. Firefox and Safari) 
  * Extracted common functions from `internal.default-pipeline-state` so they can be reused in other persistence components (#38)
  * Generalized pipeline-state-updater to be started by `assemble-pipeline` and pushes updates to any pipeline-state 
    component that is configured (#38)
  * support alternative pipeline-state component in `assemble-pipeline` (#40)
  * fixed bug where `either` didn't kill remaining steps after finishing
  * REST api endpoint `/api/builds/<buildnumber>/` now returns status 404 in case the build does not exist instead of
    returning a half empty data-structure (#42)
* API changes: 
  * Remove deprecated access to the internal pipeline-state through `:state` in the result of assemble-pipeline
  * Remove deprecated `:step-results-channel`
  * `pipeline-state-updater` now started by assemble-pipeline (see above), pipeline-state component should no longer 
    start their own update mechanism (#38)

## 0.4.3

Housekeeping release: Contains mostly cleanup under the hood and changes to APIs for advanced users.
 If you are using custom control-flow steps, runners, persistence mechanisms or other advanced features, make sure you
 look through the changes and upgrade as future releases will remove deprecated functionality.

* Improvements:
  * Now providing events `:step-result-updated` and `:step-finished` on event-bus for use by other components like
    runners and persistence components (#38)
  * Clarified interface for `core/execute-step`
* API changes:
  * The `:step-results-channel` is now deprecated, unsupported and will be removed in subsequent releases.
    Use the new `:step-result-updated` event and filter on `:step-id` to receive updates from child-steps while they run.
  * `(ui-server/ui-for pipeline-def pipeline-state ctx)`  is now deprecated and will be removed in subsequent releases.
    Use `(ui-server/ui-for pipeline)` instead.
  * Direct access to the pipeline-state atom, e.g. through `:state` in the result of assemble-pipeline is now deprecated
    and will be removed in subsequent releases. Use the event-bus or access the state through the `PipelineStateComponent`
    protocol instead.
  * Removed `default-pipeline-state/notify-when-no-first-step-is-active`.
    Use the new events (see above) to be notified about changes to the state of the pipeline

## 0.4.2

* Improvements:
  * Increased time between git-polls in `git/wait-for-git` and `git/wait-for-details` to 10 seconds and
    made that value configurable with an optional parameter `:ms-between-polls`, e.g.

    ```clojure
    (git/wait-with-details ctx some-repo-uri "master" :ms-between-polls 60000)
    ```
  * Added a feature to kill running steps on user request (#31)

## 0.4.1
* Improvements:
  * Added `junction` control flow step that adds a way to model if-then-else logic in a pipeline (#28,#32)
  * No longer shipping a `logback.xml` in the published jar.

## 0.4.0

* Improvements:
  * Cleaned up status inheritance to make it more consistent
  * Support variable arguments instead of step-vector as input for `chain-steps` (#29)
  * Internal refactorings to prepare for more component-oriented structure
  * Contexts always contain `:step-results-channel` which can be used to access a stream of step-state updates,
    e.g. to create custom persistence components or runners (still experimental)
  * Cleaned up dependencies
* Bug fixes:
  * Fix bug in retriggering that left next step in undefined state (#26)
* API changes:
  * Steps returning no `:status` will now be treated as failures instead of receiving status `:undefined`
  * Removed deprecated `:result-channel` argument for `execute-step`
  * Removed deprecated `core/new-base-context-for`
  * `core/execute-step` does no longer output the result-channel data to a `:result-channel` in ctx.
    Was replaced with `:step-results-channels` which provides a stream of complete, aggregated step-result data
  * Calling `lambdacd.steps.support/chain-steps` with a vector instead of just the steps is now deprecated (#29)

## 0.3.2

* Improvements:
  * Remove styling for undefined step-status (#23)
  * All container steps inherit their childrens status by default (#24)
  * Add a `run`-container step that can group nested steps (#21)
  * Ignore step-results cloned by retriggering in determining start and stop timestamps in pipeline history (#22)
  * UI: build-history now in descending order (i.e. recent builds first) (#25)
* API changes:
  * the `:result-channel` argument for `execute-step` is now deprecated. Pass custom result-channels in via the ctx instead
  * Generating a new base context moved to `execute-step`, therefore `core/new-base-context-for` is now deprecated, just use `ctx` instead

## 0.3.1

* Bug fixes:
  * Fix updated scrolling (#18)

## 0.3.0

* Bug fixes: #14, #15, #17
* Improvements:
 * Display status in output at the end of a build step (#4)
 * Display total build time in history (#16)
 * Indicate lost connection to LambdaCD in UI
 * Allow retriggering of nested steps (#5)
 * Redirect to new build when retriggering
 * Support hardcoded result-maps in `chain`-macro
 * Support setting environment-variables in `shell/bash` (#9)
* API Changes:
 * removed deprecated argument lists for `lambdacd.internal.execution/execute-steps`


## 0.2.3

* Bug fixes: #13

## 0.2.2

* Bug fixes: #8, #10, #11, #12
* New features:
  * `support/print-to-output` simplifies printing to the output-channel within a step
  * `support/printed-output` in return allows you to get everything that was printed within that step, typically to return at the end of a step
* Improvements:
  * `with-git` removes its workspace after all child-steps are finished
  * Display build status in history (#3)
* API Changes
  * `lambdacd.internal.execution/execute-steps` is now deprecated. use the public `lambdacd.core/execute-steps` instead. (#7)
    `lambdacd.core/execute-steps` takes keyword-arguments instead of argument lists for optional parameters.
  * `lambdacd.internal.execution/execute-step` is now deprecated. use the public `lambdacd.core/execute-step` instead. (#7)

## 0.2.1

* UI: support safari and other older browsers
* Step-Chaining now passes on the initial arguments to all the steps to enable users to pass on parameters that are
  relevant for all steps, e.g. a common working directory.
* `wait-for-git` no longer returns immediately when no last commit is known. Instead just waits for the first commit.
  This behavior seems more intuitive since otherwise on initial run, all build pipelines would start running.
* Git-Support: added `with-commit-details`, a function that enriches the `wait-for-git` result with information about the
  commits found since last build and a convenience function `wait-with-details` that wraps both.

## 0.2.0

* Recording start and update timestamps for every build step
* Cleanup old endpoints: `/api/pipeline` and `/api/pipeline-state`
* Improve retriggering: Create a new pipeline-run instead of overwriting existing builds
* Make bash-step killable
* Breaking Changes:
 * removed `core/mk-pipeline` as a method to initialize the pipeline.
   Replaced with a more flexible `core/assemble-pipeline` and a few single functions that take over things like running
   the pipeline and providing ring-handlers to access the pipeline. For details, see examples.

## 0.1.1

* Improvements in UI

## 0.1.0

* LambdaCD now requires Clojure 1.6.0

## 0.1.0-alpha13

* Major restructuring: (`scripts/migrate-to-new-package-structure.sh` can help rewrite your code to work with the new structure)
  * `control-flow`, `git`, `manualtrigger` and `shell` are now in `steps`-package
  * `lambdacd.presentation` is now `lambdacd.presentation.pipeline-structure`
  * some functions more concerned with presentation than actual management of the pipeline state are moved from `lambdacd.pipeline-state` to `lambdacd.presentation.pipeline-state`
  * internals are now in the `internal` package
  * ui related functionality is now in the `ui` package
  
## 0.1.0-alpha12

## 0.1.0-alpha11

* Removed support for steps returning channels (deprecated since alpha8)

## 0.1.0-alpha10

## 0.1.0-alpha9

## 0.1.0-alpha8

* steps now receive a `:result-channel` value through the context to send intermediate values.
  use this channel instead of returning a result-channel from the step. the latter is now DEPRECATED and will be removed in the next release.
* `bash` now needs the context as first parameter:

  ```clojure
  (shell/bash ctx cwd
      "lein test")
  ```


## 0.1.0-alpha7

no breaking API changes

## 0.1.0-alpha6

* `wait-for-git` now receives the context as first parameter: `(wait-for-git ctx some-repo some-branch)`
  The context must contain a configuration with the `:home-dir` set to a directory where the step can store a file with the last seen revisions (see below)
* `mk-pipeline` requires and additional parameter, the configuration-map (which is where you set the home-dir)

## 0.1.0-alpha5

* just a bugfix for the git-handling 

## 0.1.0-alpha4

* Pipeline-Steps can now return a core-async channel to continously update their state while the step is running (e.g for long-running steps that need to indicate progress on the UI):
 
 ```clojure 
 (async/>!! ch [:out "hello"])
 (async/>!! ch [:out "hello world"])
 (async/>!! ch [:status :success])
 (async/close! ch)
 ```
 
* Dropped support for core-async channels as `:status` value in a steps result-map. Use channels for the whole result instead (see above)

## 0.1.0-alpha3

* parameters for steps changed. the second argument is now a context-map that contains the step-id and
  other low-level information
    * previously `(defn some-step [args step-id])`
    * now `(defn some-step [args {:keys [step-id] :as ctx}])`
* `start-pipeline-thread` has been removed as the initializing function. use the new setup-functionality using `mk-pipeline`:

 ```clojure
 (def pipeline (core/mk-pipeline pipeline-def))

 (def app (:ring-handler pipeline))
 (def start-pipeline-thread (:init pipeline))
 ```
