# Preparing the development environment & Typescript project configs

- install typescript
    
    ```bash
    npm install -g typescript #install typescript
    tsc --version #show version of typescript
    ```
    
- compaile to javascript
    
    ```bash
    tsc 'file name' #compile file
    tsc -w 'file name' #compile in watch mode
    ```
    
- create `tsconfig.json` file
    
    ```bash
    tsc --init
    ```
    
    after create `tsconfig.json` file you can run `tsc -w` compile all of typescript file.
    
- create `package.json` file
    
    ```bash
    npm init -y
    ```
    
- install `nodemon` package
    
    ```bash
    npm install -g nodemon
    ```
    
    after create `packge.json` file and install `nodemon` package this command track the changes of the compiled file and re-execute if there are any changes.
    
    ```bash
    nodemon 'file name'
    ```
    
- install `typescript`, `ts-node` and `nodemon` packages
    
    ```bash
    #run separate this commands
    npm install -g typescript #local install 'npm install -D typescript'
    
    npm install -g ts-node #local install 'npm install -D ts-node'
    
    npm install -g nodemon #local install 'npm install -D nodemon'
    ```
    
- run typescript file directly with `ts-node` package
    
    after create `packge.json` file and install `typescript`, `ts-node` and `nodemon` packages this command run typescript file directly.
    
    ```bash
    ts-node 'file name'
    ```
    
    after create `packge.json` file and install `typescript`, `ts-node` and `nodemon` packages this command track change of typescript file and rerun the previous command.
    
    ```bash
    nodemon --exec "ts-node" 'file name'
    ```
    
    create script in `packge.json` file and run using `npm`  or `yarn`.
    
    ```json
    {
      "scripts": {
        "start": "nodemon --exec \"ts-node\" 'file name' "
      }
    }
    ```