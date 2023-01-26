# Monotonic Versioning Manifesto 1.2

*Note: This is a copy of [the original](http://blog.appliedcompscilab.com/monotonic_versioning_manifesto/), which has since gone offline. [Wayback machine snapshot here.](https://web.archive.org/web/20210208173000/http://blog.appliedcompscilab.com/monotonic_versioning_manifesto/)*

## Summary
A version is composed to two numbers: `COMPATIBILITY.RELEASE` where

`COMPATIBILITY` - a number representing the line of compatibility of the release.
`RELEASE` - the release number, always increasing.
Additional metadata can be added with `+` followed by dot separated, non-empty, identifiers after the `RELEASE` number.

For compatibility with Semantic Versioning, a `.0` may come after the `RELEASE` number.

## Introduction
The world of versioning is full of well-defined and ad-hoc methodologies. SemVer has risen to be one of the more popular methodologies, however I believe that SemVer encodes too much information, some of which does not actually matter for the most part. The distinction between a `MINOR` and a `PATCH`, for example, can be difficult to reason about.

Monotonic Versioning attempts to address this by simplifying the version to two numbers, neither of which are ever reset to 0. The only number which the release engineer has control over is `COMPATIBILITY`, which needs to be incremented if a new line of compatibility is added. The `RELEASE` number is incremented for every release. Releases can be totally ordered by the `RELEASE` number and compatibility between any set of releases can be determined by comparing the `COMPATIBILITY` numbers. The `COMPATIBILITY` number, which is used to indicate if two released are guaranteed to be compatible with each other, may be different but still compatible with each other, however this does not represent a guarantee, just an coincidence.

## Monotonic Versioning Specification
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

1. Software using Monotonic Versioning MUST declare a public API. This API could be declared in the code itself or exist strictly in documentation. However it is done, it should be precise and comprehensive.
2. A normal version number MUST take the form `X.Y` where `X`, `Y` are non-negative integers, and MUST NOT contain leading zeroes. `X` is the compatibility number and `Y` is the release number. Each element MUST increase numerically. For instance: `1.9` -> `1.10` -> `1.11`.
3. Once a versioned package has been released, the contents of that version MUST NOT be modified. Any modifications MUST be released as a new version.
4. Release number `Y` (`x.Y`) MUST be incremented on every release. A release number MUST be incremented by at least 1. The release number SHALL NOT ever be decremented.
5. Compatibility number `X` (`X.y`) MUST be incremented if a release is not compatible with an existing release. Compatibility is defined by the documented semantics of a an API changing in a way that would break existing uses or portions of the API being removed. The release number SHALL NOT be decremented. For instance, the following sequence of releases: `1.0` -> `1.1` -> `2.2` -> `2.3` -> `1.4` -> `2.5`
6. The release number MAY be followed by a `.0` for compatibility with Semantic Versioning. For instance: `1.9` and `1.9.0` are the same Monotonic Version.
7. Metadata MAY be denoted by appending a plus sign and a series of dot separated identifiers immediately following the release number or the SemVer-compatibility patch number. Identifiers MUST comprise only ASCII alphanumerics and hyphen: `[0-9A-Za-z-]`. Identifiers MUST NOT be empty. Examples: `1.0+001`, `1.0+20130313144700`, `1.0+exp.sha.5114f85`.
8. Precedence refers to how versions are compared to each other when ordered. Versions MUST be calculated by separating a version into compatibility, release, and metadata identifiers. Precedence is determined by the first difference when comparing each of these identifiers from left to right as follows: compatibility, release, metadata. Compatibility and release are numerically compared, where a larger number denotes a higher precedence. Metadata is compared lexically with the later sorted values having precedence.

## FAQ
1. *Why is there no pre-release versioning in Monotonic Versioning?*

The pre-release behaviour of SemVer offers little semantic information about a release. Rather than pre-releases, Monotonic Versioning simply increments the compatibility number when a release breaks compatibility with an existing one.

2. *What if I want my API version numbers to be low?*

That is silly, aesthetics have little place in versions where the point is to convey semantic information. If you want lower version numbers, get your API right earlier.

3. *What parts are compatible SemVer?*

Monotonic Versions without metadata as well as those with metadata that has no numeric-only entries have the same precedence rules as SemVer. All Monotonic Versions can be written with three numeric values for SemVer compatibility.

4. *Are bug fixes that change behaviour of an API a backwards breaking change?*

Such a change is NOT a backwards breaking change if it is bringing the API in alignment with the documentation. If the API was never documented then exercise common sense about what will surprise your users the least if the API is backwards breaking or not.

5. *What should I call this for short?*

I'm leaning towards MonoVer, however MVer is a possibility as well as MoVer.

## Visualization
[@asthasr](https://twitter.com/asthasr) created an example flow here (TODO: original unavailable).

* Colored arrows are compatibility
* Solid gray arrows are chronology
* Dashed arrows are source code evolution (backports, etc.)

## Acknowledgments
This is a modified version of the Semantic Versioning Specification. Thank you to Tom Preston-Werner for creating it.

## License
This work is licensed under the Creative Commons Attribution 3.0 Unported License. To view a copy of this license, visit http://creativecommons.org/licenses/by/3.0/ or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.

## Credits
Author: orbitz

Updated: 2016-06-25 Sat 16:58
