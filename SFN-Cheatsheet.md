[back to overview](/../..)

# SFN Cheatsheet<!-- omit in toc -->

- [SFN Build and Deployment](#sfn-build-and-deployment)
  - [Dev:](#dev)
  - [Yarn:](#yarn)
  - [remix:](#remix)
  - [CI:](#ci)

## SFN Build and Deployment

### Dev:

  - `dev server` -> `yarn dev`
  - `dev up` -> `yarn db:migrate` && secrets download from gcloud
  - `SHOPIFY_CLI_1P_DEV=1 yarn dev`

### Yarn:

  - `yarn build` -> `remix build`
  - `yarn dev` -> `shopify app dev` -> `/shopify.web.toml` `dev` -> `yarn remix:dev`
  - `yarn start` -> `node build/index.js`
  - `yarn deploy` -> `shopify app deploy`
  - `yarn remix:dev` -> `./scripts/run-app.sh` -> if prod {`yarn remix:prod`} else {`remix dev -c "ts-node -r tsconfig-paths/register server/dev.ts"`}
  - `yarn remix:prod` -> `yarn build && yarn start`

### remix:
  - `remix build` -> set `process.env.NODE_ENV=production` && build app for production using remix compiler
  - `remix dev` -> set `process.env.NODE_ENV=development` && runs remix compiler in watch mode -> -c runs ts-node app server
  - `server/dev.ts` -> custom app server on express for development -> `build/index.js`
  - `server/prod.ts` -> custom app server on express for production
  - `remix.config.js` -> if prod { set server to `server/prod.ts` && serverBuildPath to `build/index.js` }

### CI:
  - merge to main -> `build-and-deploy.yml`
    - deploy -> `prod-kit/actions/deploy`
      - Procfile `pre-deploy`: Cloud Run job -> `yarn db:migrate`
      - deploy code revision to Cloud run
      - Procfile `web`: `yarn start`
    - deploy-extensions -> `deploy-extensions.yml`
  - `deploy-extensions.yml`
    - build -> `yarn build`
    - deploy -> `yarn deploy -f`
