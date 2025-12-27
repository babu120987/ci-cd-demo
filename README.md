echo "# ci-cd-demo" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/babu120987/ci-cd-demo.git
git push -u origin main
