# Fixes applied to this copy

This copy contains source-level fixes for deployment and parsing reliability.

## Environment variables

Frontend:
- `VITE_API_URL` — public FastAPI URL when frontend/backend are separate. Leave empty for same-origin/proxy deployments.
- `VITE_DEV_API_TARGET` — local Vite proxy target (default `http://127.0.0.1:8001`).
- `VITE_MAX_FILE_SIZE_MB` — frontend upload limit.

Backend:
- `HOST` / `API_HOST`
- `FASTAPI_PORT` / `API_PORT`
- `ENVIRONMENT`
- `LOG_LEVEL`
- `REDIS_URL`
- `CELERY_BROKER_URL`
- `CELERY_RESULT_BACKEND`
- `UPLOAD_DIR`
- `RESULTS_DIR`
- `SAMPLES_DIR`
- `MAX_FILE_SIZE_MB`
- `CONFIDENCE_THRESHOLD`
- `CORS_ORIGINS`

## Main code fixes

1. Removed hard-coded 25 MB validation from the backend parser and upload endpoint.
2. Made frontend upload validation use `VITE_MAX_FILE_SIZE_MB`.
3. Made frontend API requests use `VITE_API_URL` when supplied.
4. Made the Vite development proxy configurable.
5. Made `run_services.sh` honor the configured FastAPI port.
6. Made Docker Redis/Celery result backend use Redis DB 1 consistently.
7. Removed the fake fallback PDF returned when a requested sample does not exist.
8. Invalid uploaded files are removed after signature validation fails.
9. Added full-page PDF OCR fallback using `pdf2image` + Poppler for scanned PDFs that have no usable embedded images.
10. Removed the DOCX heuristic that treated every 25 blocks as a page; explicit Word page-break markers are used instead.
11. Removed fabricated image-parser text when OCR finds no text.
12. Empty extraction confidence is now 0% rather than an invented 90%.
13. CORS no longer uses wildcard origins with credentials enabled.

## Validation performed

- Python source compilation: passed.
- Parser smoke tests using included samples: passed.
- PDF sample: parsed successfully.
- Application PDF sample: parsed successfully.
- PNG sample with OCR: parsed successfully.
- Full pytest suite: not runnable in this sandbox because `redis` and `celery` packages are not installed.
- Frontend TypeScript/build: not runnable because npm dependencies could not be downloaded in this sandbox.
