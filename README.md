# lyrics-highlight-app

A Vue.js application for displaying and highlighting song lyrics. Users can navigate through different songs and highlight specific lines of lyrics.

## Project setup

### Prerequisites
- Node.js (>= 12.x)
- Yarn (>= 1.x)

### Installation
1. Clone the repository:
    ```sh
    git clone https://github.com/your-username/lyrics-highlight-app.git
    cd lyrics-highlight-app
    ```

2. Install dependencies:
    ```sh
    yarn install
    ```

### Compiles and hot-reloads for development
```sh
yarn serve
```

### Compiles and minifies for production
```sh
yarn build
```

### Lints and fixes files
```sh
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

## Usage

1. Start the development server:
    ```sh
    yarn serve
    ```

2. Open your browser and navigate to `http://localhost:8080`.

3. Use the following keyboard controls to interact with the app:
    - `Space`: Highlight the next lyric line.
    - `ArrowRight`: Load the next song.
    - `ArrowLeft`: Load the previous song.
    - `ArrowUp`: Highlight the previous lyric line.
    - `ArrowDown`: Highlight the next lyric line.
    - `F5`: Toggle presentation mode.

## Project Structure

```
.
├── .gitignore
├── babel.config.js
├── jsconfig.json
├── package.json
├── public
│   ├── assets
│   │   ├── song_1.json
│   │   ├── song_2.json
│   │   └── song_3.json
│   └── index.html
├── README.md
├── src
│   ├── App.vue
│   ├── assets
│   ├── components
│   │   └── HelloWorld.vue
│   └── main.js
└── vue.config.js
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
