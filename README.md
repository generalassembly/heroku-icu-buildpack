# Heroku ICU Buildpack

A buildpack for including the ICU library into a Heroku dyno. This is required
as a dependency for several tools.

## How it works

This buildpack makes ICU4C available in the `/app/icu` directory for both
compilation and runtime uses. The primary purpose of this is to allow [Charlock Holmes](https://github.com/brianmario/charlock_holmes)
(a Ruby gem for encoding heuristics) to run on Heroku.

In order to achieve this, the following happens:

1. ICU is downloaded and compiled from source
2. The compiled library is cached for future builds
3. The compiled result is copied into `/app/icu` and `$1/icu` (the build
   directory) to make it available in a seemingly consistent location during
   both compilation and runtime
4. A Bundler config option is exported to the rest of the compilation process to
   ensure Charlock Holmes looks for ICU in the correct place.
