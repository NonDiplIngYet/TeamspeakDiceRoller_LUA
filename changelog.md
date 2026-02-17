# Merged Changelog (alick + nullArc)

-- From `alick-changes/changelog.md` --

## Beta 1.5
Colors for everyone!
###
- User can now set colors for themselves
- Implemented !treffer for DSA ("DSA Trefferzonen")
- Refactorings to reduce code duplication and optimize readability
- Fixes some bugs by replacing them with brand new ones.

## Beta 1.4.1
Fixing DSA (as usual for 4.1)
### Features
- more user colours / unique IDs
- different modes (rule systems) now covered in an if-elseif-else-wrapper for cleaner running
- now "simple" single stat checks in DSA also can have boni or mali (as they're supposed to!)
- Surprisingly severe refactoring of the DSA mode code section to facilitate that
- succeeding a DSA skill check with 0 skill points remaining will round up to 1 point as supposed to by the rules


-- From `nullArc-changes/changelog.md` --

## Beta 1.4.3

### Features
- generic rolls mode now supports 2 different types of generic rolls:
- use !<number>,<die>,<modifier> in Generic Mode to roll <number>D<die> & add them up together with an optional <modifier> (If only 1 number given, it'll roll 1 Die with <input> sides)
- use ?<number>,<die>,<threshold> in Generic Mode to roll <number>D<die> & count all dice scoring <threshold> or above as sucesses (also counts 1s separately)
- minor refactoring to open for broader syntax use

## Beta 1.4.2
Colors for everyone!
###
- User can now set colors for themselves
- Implemented !treffer for DSA ("DSA Trefferzonen")
- Refactorings to reduce code duplication and optimize readability
- Fixes some bugs by replacing them with brand new ones.
