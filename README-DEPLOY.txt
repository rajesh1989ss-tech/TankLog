TANK LOG 1.1.0  —  EXCEL IMPORT + SETTINGS REORGANISED
======================================================
App version .......... 1.1.0
schemaVersion ........ 2       (UNCHANGED — no migration, no re-sync needed)
Service-worker cache . tanklog-v1.1.0
Windows exe .......... NOT BUILT HERE — re-run windows\build_exe_py312.bat
Tests ................ 196 + 37 + 55 = 288 assertions, all passing


-------------------------------------------------------------------
UPLOAD TO GITHUB:  index.html  and  sw.js       (that is all)
WINDOWS FOLDER:    main.py, build_exe_py312.bat, icon.ico, index.html
-------------------------------------------------------------------


===================================================================
1. EXCEL IMPORT NOW ACCEPTS ANY CATEGORY
===================================================================
Settings -> Spaces & Categories -> Space List (Paste from Excel)

Before, the Category column only accepted CARGO / BALLAST / FW / COFF.
Anything else — Stores, Rafts and Boats, Void Spaces — was thrown out with
"new tank needs valid category & side". That was the whole problem.

CATEGORY IS NOW FREE TEXT.
  * CARGO, BALLAST, FW, COFF still map to the built-ins.
  * An existing category matches on its id, its short code OR its full label,
    ignoring case and punctuation — "rafts and boats", "Rafts And Boats" and
    "RAFTSANDBOATS" all find the same one.
  * Anything else creates a NEW CATEGORY automatically. Add as many as you
    like straight from the sheet; there is no limit.

The preview tells you exactly what will happen before anything is written:

    ＋ NEW CATEGORY — Rafts And Boats (RAB, 2 spaces)
    ＋ NEW CATEGORY — Stores (STORES, 2 spaces)
    ＋ NEW Life Raft (P) — Rafts And Boats · PORT
    ✓ COT 3 (P): category → Stores
    • COT 1 (P) — unchanged (logs kept)
    ! Void Space 1 — no side given, set to FULL WIDTH
    ✗ row 7 — duplicate name, ignored: Bosun Store

Labels are tidied to Title Case and a short report code is derived from the
initials (Rafts And Boats -> RAB), made unique if it clashes. Rename either
one afterwards in Settings -> Spaces & Categories -> Categories.

MISSING CELLS NO LONGER REJECT THE ROW:
  * Side blank or unrecognised -> FULL WIDTH, and the preview says so in amber.
  * Category blank on a NEW space -> filed under your first category, flagged.
  * Category blank on an EXISTING space -> left exactly as it is.
  * PORT/P/PT, STBD/S/SB/STARBOARD, FULL/C/CL/CENTRE/MIDDLE all understood.
  * Comma-separated rows work as well as tab-separated.

Still true, and still tested:
  * Existing spaces are matched by name — LOGS ARE ALWAYS KEPT.
  * NOTHING IS EVER DELETED by this import.
  * Importing the same sheet twice changes nothing.
  * Duplicate names within one paste are reported with their row number.

"Copy current space list" now exports your real category labels grouped in
dashboard order, so it round-trips: copy out, edit in Excel, paste back.


===================================================================
2. SETTINGS REORGANISED
===================================================================
Eighteen blocks in one long scroll became seven collapsible groups, all
closed when you open Settings, plus a search box at the top.

    ▦  Spaces & Categories   add, rename, reorder, import from Excel
    ⚓  Vessel & Voyage       identity, voyage and leg records
    ✎  Entry & Measurement   note sections, quick-picks, deviation limits
    ≡  Bulk Data from Excel  tank data and last-3-cargoes paste
    ◐  Appearance            theme and display
    ⤓  Backup, Sync & Trash  export, import, snapshots, deleted, change log
    ⚠  Master Options        restricted actions

  * Tap a heading to open that group; it scrolls itself into view.
  * Each heading shows how many blocks are inside.
  * Type in the search box and only matching groups show, already opened.
    "Expand all" becomes "Clear" while a search is active.
  * Groups whose blocks are all Master-only hide themselves until you are in
    Master mode, so the list is shorter when you are not.

Nothing was removed or renamed — everything is where it was, just filed.


===================================================================
3. ALSO FIXED
===================================================================
* The import preview kept showing its previous result after the paste box was
  emptied, so a second preview could look like it had run when it had not.
* "Copy current tank list" wrote hard-coded category names, so custom
  categories came out wrong. It now uses the real labels.
* Wording throughout the importer now says "space" rather than "tank", to
  match the rest of the app.


===================================================================
4. UPDATING
===================================================================
PHONE
  1. Upload index.html AND sw.js to the repo root, replacing the old ones.
     Both. The cache name changed (tanklog-v1.0.1 -> tanklog-v1.1.0) and
     without the new sw.js the phone keeps serving the old code.
  2. Swipe Tank Log out of recents, reopen it.
  3. Settings should read v1.1.0.

  Still on the old version? Chrome -> Settings -> Site settings -> your
  github.io address -> Clear & reset, then reopen. Your data is not in the
  service-worker cache, so that is safe.

DESKTOP
  Drop the new index.html into the build folder, run build_exe_py312.bat,
  copy dist\TankLog.exe over the one on the USB stick. TankLog.db, backups\
  and webdata\ sit beside the exe and survive the swap.

NEXT TIME YOU CHANGE THE HTML
  1. Bump APP_VERSION near the top of the script block in index.html.
  2. Change the CACHE string in sw.js.
  3. Upload index.html + sw.js to GitHub.
  4. Same index.html into the build folder, run the .bat.
  5. Copy the new exe onto the stick — the build only writes to dist\.


===================================================================
5. TESTS
===================================================================
All three suites drive the real DOM — clicks, typing, the import file picker,
history.back() for the Android gesture — rather than reaching into the app's
private scope, so what passes is what you can actually do on the phone.

    npm install jsdom
    node smoke.js         ->  196 passed
    node smoke-back.js    ->   37 passed
    node smoke-import.js  ->   55 passed

smoke.js         boot and migration, collapsible sections, age badges, quick
                 jump, master mode, category manager, space reorder,
                 before/after entries, bulk entry, calendar scope and date
                 range, presets, report cover page, print discipline, export
                 shape, two-way merge, idempotent re-import, newest-wins,
                 soft-delete propagation, schema guard, legacy upgrade.
smoke-back.js    one, two and three stacked layers, mixed button-and-back
                 dismissal, lightbox as innermost layer, Esc parity, back on
                 the bare dashboard.
smoke-import.js  settings grouping and search, free-text category creation,
                 label and short-code derivation, missing side and category
                 defaults, new categories reaching the dashboard, idempotent
                 re-import, duplicate detection, copy-current round-trip.

Keep all three next to index.html on the PC and re-run after every change.
