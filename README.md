# Introduction to Algorithms Course Materials

This repository contains the (WIP) teaching materials for the Introduction to Algorithms module, designed while being the TA for University College Dublin Computer Science Module COMP20290 Algorithms.
The content is structured as an MD-book and is automatically deployed to GitHub Pages.

## Repository Structure

```
.
├── src/              # Source markdown files for the course content
├── theme/            # Custom theme configurations
├── book.toml         # mdBook configuration file
└── .github/
    └── workflows/    # GitHub Actions workflow for deployment
```

## Prerequisites

To work with this repository, you'll need:

1. [Rust](https://www.rust-lang.org/tools/install) installed on your system
2. [mdBook](https://rust-lang.github.io/mdBook/guide/installation.html) installed via cargo:
   ```bash
   cargo install mdbook
   ```

## Local Development

1. Clone the repository:
   ```bash
   git clone https://github.com/jmg049/Introduction-To-Algorithms.git
   cd intro-to-algorithms
   ```
2. Build the book locally:
   ```bash
   mdbook build
   ```
3. Serve the book locally with auto-reload:
   ```bash
   mdbook serve --open
   ```
   This will start a local server and open the book in your default web browser. The content will automatically reload when you make changes to the source files.

## Making Changes

1. Edit the markdown files in the `src` directory
2. Preview changes locally using `mdbook serve`
3. Commit and push your changes to the main branch
4. The GitHub Actions workflow will automatically build and deploy the site

## GitHub Pages Deployment

The repository is configured to automatically deploy to GitHub Pages using GitHub Actions. The workflow:

1. Triggers on pushes to the main branch
2. Builds the mdBook
3. Deploys to the `gh-pages` branch

The site is available at: [Introduction to Algorithms](https://jmg049.github.io/Introduction-To-Algorithms/)

### Troubleshooting Deployments

If the deployment fails:
1. Check the Actions tab in the GitHub repository for error messages
2. Ensure the repository settings allow GitHub Actions
3. Verify that the Pages settings are configured to deploy from the `gh-pages` branch

## Content Maintenance Guidelines

When updating the course materials:

1. Try and maintain consistent formatting across all markdown files
2. Use relative links for internal references
3. Test all code examples before committing
4. Update the `SUMMARY.md` file when adding new chapters
5. Try and add addtional Java specific resources for whatever is used as part of teaching. 

## Additional Resources

- [mdBook Documentation](https://rust-lang.github.io/mdBook/)
- [Markdown Guide](https://www.markdownguide.org/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)

## Support

For any questions or issues:
1. Create an issue and I will see it :)

## License

MIT License

Copyright (c) 2021 ZJPzjp

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---
