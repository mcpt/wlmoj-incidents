# Incident 2020-09-11: PDF Generation Disrupted

## Issue Summary
Memory starvation on sites during PDF generation

## Timeline (Eastern Time)
- `2020-09-11 18:28` First error of "Cannot allocate enough memory"

## Root Cause
PDF generation is a memory intensive process because it spawns a chromium browser. Clicking on the PDF generation link multiple times for a problem without a cached PDF uses up all available resources.

## Resolution and recovery
None.

## Corrective and Preventative Measures
Keeping track of whether a problem is having a PDF generated may be possible, but this isn't that big of an issue.
