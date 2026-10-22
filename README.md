# TA Gradebook Filter

A browser bookmarklet that filters a gradebook table by a list of students, arranges matching rows in the order of that list, and optionally displays a single lab or assignment column.

## Features

- Load student identifiers from a local `.txt` file, one per line.
- Match identifiers against student row text, ignoring case and blank lines.
- Hide unmatched student rows and reorder matches to follow the input list.
- Show all columns or select one lab/assignment while keeping identifying columns visible.
- Display the number of matched entries after applying the filter.

## Setup

1. Open `TA Gradebook Filter.js` in a text editor and copy its entire contents, including the `javascript:` prefix.
2. Create a bookmark in your browser.
3. Name it **TA Gradebook Filter** and paste the copied code into the bookmark's URL/location field.
4. Save the bookmark, ideally in your bookmarks bar.

This file is a bookmarklet: run it from a bookmark while viewing the gradebook, rather than with Node.js or by opening the file directly.

## Prepare your student list

Create a plain-text file such as `students.txt`, with one identifier per line:

```text
student1
student2
student3
```

A username alone is enough; you do not need to include `@` or an email domain. You can also use a complete email address or student ID, provided it appears in the student row. Use unique identifiers. Matching uses a **substring search across the entire row**, so short or ambiguous entries may select the wrong student. Only the first matching row is selected for each entry.

## Usage

1. Open your gradebook's **Grader report** page and ensure the intended students are loaded in the table.
2. Click the **TA Gradebook Filter** bookmark.
3. Select your `.txt` student list.
4. In **Filter Students & Columns**, choose a lab/assignment or leave **Show All Columns** selected.
5. Click **Apply Filter & Sort**.

Matching rows appear in the order specified in the text file. Click **Cancel** to close the dialog without applying changes.

The bookmarklet can run only once per page load. Any further attempt displays a message asking you to refresh the page. Refresh to run it again and restore the original rows, columns, and ordering. This also applies if you cancel the file picker or filter dialog.

## How it works

The script reads the text file with the browser's `FileReader` API and modifies the current page's table elements. It identifies the last row containing more than three header cells as the header, and treats body rows containing an `@` character or an input element as student rows.

When a specific assignment is selected, it preserves the first three columns and headers containing `name`, `email`, `id number`, or `student id`, alongside the selected column.

## Limitations and troubleshooting

- Table detection depends on the page structure and English header labels. Other layouts, merged headers, or translated interfaces may require code changes.
- Only rows currently loaded in the page are considered; the script does not fetch other pages of students.
- If **Grader report table not found.** appears, check that you are on the correct report page with student rows loaded.
- Duplicate identifiers can count the same row more than once. The matched count is the number of successful list-entry matches, not necessarily the number of distinct students.
- Browser or site restrictions may prevent bookmarklets from running.

## Data and grade changes

The script contains no network requests and reads the selected file locally in the browser. Its direct actions change row order and visibility on the current page; it does not edit grade values or submit the gradebook form.
