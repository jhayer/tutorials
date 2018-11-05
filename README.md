#Tutorials

Collection of tutorials on various bioinformatics topics

##Dev

First clone the repository and install mkdocs and the theme using pipenv

```
git clone https://github.com/HadrienG/tutorials.git
cd tutorials
pipenv install
```

For a live preview in your browser do

```
pipenv run dev
```

##Deploy

The following command will build and push your website to a gh-pages branch. Only do this if you want your own version of the website! If you are modifying the original, please open a pull request.

```
pipenv run deploy
```
