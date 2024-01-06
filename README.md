# Angular Spotify

A simple Spotify client built with Angular 15, Nx workspace, ngrx, TailwindCSS and ng-zorro.


**Spotify premium** is required for the Web Playback SDK to play music. If you are using a free account, you can still browse the app, but it couldn't play the music. Sorry about that 🤣

## Support

If you like my work, feel free to:

- ⭐ this repository. And we will be happy together :)

## Who is it for 🤷‍♀️

I still remember Window Media Player on windows has the visualization when you start to play the music, and I wanted to have the same experience when listening to Spotify. That is the reason I started this project.

I found that there weren't many resources on building a proper real-world scale application, and that's my focus for sharing. I hope Angular Spotify will give you more insight on that even though it is my first project using Nx 🤣

## Tech stack

[angular]: https://angular.io/
[ngrx]: https://ngrx.io/
[component-store]: https://ngrx.io/guide/component-store
[tailwind]: https://tailwindcss.com/
[ng-zorro]: https://ng.ant.design/docs/introduce/en
[netlify]: http://netlify.com/
[ngneat]: https://github.com/ngneat

I experimented with the ngrx/component store for `AuthStore` and `UIStore`. It might not be a best practice, and I will refactor it very soon. Just FYI 🤣

### Principles

All components are following:

- OnPush Change Detection and async pipes: all components use observable and async pipe for rendering data without any single manual subscribe. Only some places are calling subscribe for dispatching an action, which I will have a refactor live stream session with my friend [@nartc][nartc] to use the component store for a fully subscribe-less application.
- SCAMs (single component Angular modules) for tree-shakable components, meaning each component will have a respective module. For example, a RegisterComponent will have a corresponding RegisterModule. We won't declare RegisterComponent as part of AuthModule, for instance.
- Mostly, everything will stay in the `libs` folder. New modules, new models, new configurations, new components etc... are in libs. libs should be separated into different directories based on existing apps. We won't put them inside the apps folder. For example in an Angular, contains the `main.ts`, `app.component.ts` and `app.module.ts`

### Structure

I followed the structure recommended by my friend [@nartc][nartc]. Below is the simplified version of the application structure.

```
.
└── root
    ├── apps
    │   └── angular-spotify
    └── libs
        └── web (dir)
            ├── shell (dir)
            │   ├── feature (angular:lib) - for configure any forRoot modules
            │   └── ui
            │       └── layout (angular:lib)
            ├── settings (dir)
            │   ├── feature (angular:lib) - for configure and persist all settings
            │   └── data-access (workspace:lib) - store and services for settings module
            ├── playlist (dir)
            │   ├── data-access (angular:lib, service, state management)
            │   ├── features
            │   │   ├── list (angular:lib PlaylistsComponent)
            │   │   └── detail (angular:lib PlaylistDetailComopnent)
            │   └── ui (dir)
            │       └── playlist-track (angular:lib, SCAM for Component)
            ├── visualizer (dir)
            │   ├── data-access (angular:lib)
            │   ├── feature
            │   └── ui (angular:lib)
            ├── home (dir)
            │   ├── data-access (angular:lib)
            │   ├── feature (angular:lib)
            │   └── ui (dir)
            │       ├── featured-playlists (angular:lib, SCAM for Component)
            │       ├── greeting (angular:lib, SCAM for Component)
            │       └── recent-played (angular:lib, SCAM for Component)
            └── shared (dir)
                ├── app-config (injection token for environment)
                ├── data-access (angular:lib, API call, Service or State management to share across the Client app)
                ├── ui (dir)
                ├── pipes (dir)
                ├── directives (dir)
                └── utils (angular:lib, usually shared Guards, Interceptors, Validators...)
```

### Authentication Flow

I follow `Implicit Grant Flow` that Spotify recommended for client-side only applications and did not involve secret keys. The issued access tokens are short-lived, and there are no refresh tokens to extend them when they expire.

View the [Spotify Authorization Guide](https://developer.spotify.com/documentation/general/guides/authorization-guide/)

- Upon opening Angular Spotify, It will redirect you to Spotify to get access to your data. Angular Spotify only uses the data purely for displaying on the UI. We won't store your information anywhere else.
- Angular Spotify only keeps the access token in the browser memory without even storing it into browser local storage. If you do a hard refresh on the browser, It will ask for a new access token from Spotify. One access token is only valid for **one hour**.
- After having the token, I'll try to connect to the Web Playback SDK with a new player - `Angular Spotify Web Player`

![Angular Spotify Web Playback SDK flow][sdk-flow]

### Dependency Graph

Nx provides an [dependency graph][dep-graph-nx] out of the box. To view it on your browser, clone my repository and run:

```bash
npm run dep-graph
```

A simplified graph looks like the following. It gives you insightful information for your mono repo and is especially helpful when multiple projects depend on each other.

![Angular Spotify Dependency Graph][dep-graph]

### Nx Computation Cache

Having Nx Cloud configured shortens the deployment time quite a lot.

Nx Cloud pairs with Nx in order to enable you to build and test code more rapidly, by up to 10 times. Even teams that are new to Nx can connect to Nx Cloud and start saving time instantly. Visit [Nx Cloud](https://nx.app/) to learn more.

![Nx cloud][nx-cloud]

## Features and Roadmap

I set a tentative deadline for motivating myself to finish the work on time. Otherwise, It will take forever to complete :)

## Setting up the development environment 🛠

- `git clone https://github.com/trungk18/angular-spotify.git`
- `cd angular-spotify`
- `npm start` for starting Angular web application
- The app should run on `http://localhost:4200/`

## License

Feel free to use my code on your project. Please put a reference to this repository.

[MIT](https://opensource.org/licenses/MIT)

[issues]: https://github.com/trungk18/angular-spotify/issues/new
[pull]: https://github.com/trungk18/angular-spotify/compare
[jira]: https://jira.trungk18.com/
[twitter]: https://twitter.com/trungvose
[nx]: https://nx.dev/
[angularsg]: https://twitter.com/angularsg
[angularvn]: https://twitter.com/ngvnofficial
[nartc]: https://github.com/nartc
[gist]: https://gist.github.com/trungk18/7ef8766cafc05bc8fd87be22de6c5b12
[dep-graph-nx]: https://nx.dev/latest/angular/structure/dependency-graph
[stack]: /libs/web/shared/assets/src/assets/readme/angular-spotify-tech-stack.png
[time]: /libs/web/shared/assets/src/assets/readme/time-spending.png
[dep-graph]: /libs/web/shared/assets/src/assets/readme/dep-graph.png
[sdk-flow]: /libs/web/shared/assets/src/assets/readme/sdk-flow.png
[demo]: /libs/web/shared/assets/src/assets/readme/angular-spotify-demo-short.gif
[visualizer]: /libs/web/shared/assets/src/assets/readme/angular-spotify-visualization.gif
[angular-spotify-browse]: /libs/web/shared/assets/src/assets/readme/angular-spotify-browse.gif
[album-art]: /libs/web/shared/assets/src/assets/readme/angular-spotify-album-art.gif
[pwa]: /libs/web/shared/assets/src/assets/readme/angular-spotify-pwa.gif
[web-player]: /libs/web/shared/assets/src/assets/readme/angular-spotify-web-player.png
[nx-cloud]: /libs/web/shared/assets/src/assets/readme/nx-cloud.png