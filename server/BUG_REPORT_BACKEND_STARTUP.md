# [BUG] Backend fails to start locally due to TypeScript auth typing and Prisma 7 initialization errors

## Description
Running the backend locally fails before the server can start. Two blocking issues were observed during setup and API testing:
1. TypeScript compile errors for authUserId on Express Request.
2. PrismaClient initialization error requiring valid PrismaClientOptions with Prisma 7.

This prevents protected endpoint testing in Postman until code/config fixes are applied.

## Steps to Reproduce
1. Navigate to the server directory.
2. Install dependencies with npm install.
3. Configure environment variables in .env, including DATABASE_URL, CLERK_PUBLISHABLE_KEY, and CLERK_SECRET_KEY.
4. Run npx ts-node src/index.ts.
5. Observe startup failure:
   1. First failure: TS2339 authUserId does not exist on Request.
   2. After type fix, second failure: PrismaClientInitializationError requiring non-empty PrismaClientOptions.

## Expected Behaviour
The backend should start successfully and listen on localhost so endpoints like /health and /api/auth/me can be tested.

## Actual Behaviour
The process exits during startup with compile/runtime errors and the API is unavailable.

## Screenshots / Videos
Dashboard and network screenshots captured during auth token setup and Clerk hosted flow testing are available from the debugging session.

## Environment
- Device: Desktop
- OS: Linux
- Browser: Firefox (for Clerk dashboard) and Postman desktop client for API calls
- App Version: 1.0.0 (server package)

## Additional Context
Observed error outputs included:
- TS2339 Property authUserId does not exist on Request in src/index.ts.
- PrismaClientInitializationError from @prisma/client runtime indicating PrismaClient must be constructed with valid options in Prisma 7.
- Clerk auth itself was validated later via /api/auth/me returning a valid userId once startup issues were resolved.
