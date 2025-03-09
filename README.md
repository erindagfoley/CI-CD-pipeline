# CI/CD Pipeline Application

This application automates a Continuous Integration and Continuous Deployment (CI/CD) pipeline for your project. It ensures that all component tests pass before allowing merges into the `develop` branch and subsequently into the `main` branch. Once merged into `main`, the application automatically deploys to [Render](https://render.com/).

## Features

- **Component Testing**: Runs component tests on every pull request to the `develop` branch.
- **Branch Protection**: Ensures that pull requests can only be made to the `develop` branch and are only merged if all tests pass.
- **Auto-Deployment**: Automatically deploys to Render when changes are merged into the `main` branch.
- **CI/CD Workflow**: Streamlines the development process by automating testing, merging, and deployment.

## Prerequisites

Before using this application, ensure you have the following:

- A GitHub repository for your project.
- A Render account and a configured web service for deployment.
- Node.js and npm installed (if using a Node.js-based project).
- A testing framework (e.g., Cypress) for component tests.

## Setup

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/your-repo.git
   cd your-repo
   ```

2. **Install Dependencies**:
   If your project uses Node.js, install the required dependencies:
   ```bash
   npm install
   ```

3. **Configure GitHub Actions**:
   - Create a `.github/workflows/test-on-pr.yml` file in your repository.
   - Add the following workflow configuration:

     ```yaml
        name: Component tests
        on: 
        pull_request: 
            branches: 
            - develop
        jobs:
        cypress-run:
            runs-on: ubuntu-24.04
            steps:
            - name: Checkout
                uses: actions/checkout@v4
            - name: Cypress run
                uses: cypress-io/github-action@v6
                with:
                component: true
     ```

    - Create a `.github/workflows/test-on-pr.yml` file in your repository.
    - Add the following workflow configuration:

     ```yaml
        name: Deploy to Render on Merge to Main

        on:
        push:
            branches:
            - main

        jobs:
        deploy:
            name: Deploy to Render

            runs-on: ubuntu-latest

            steps:
            - name: Checkout code
                uses: actions/checkout@v4

            - name: Install dependencies
                run: npm install

            - name: Run tests
                run: npm run test-component

            - name: Deploy to Render
                env:
                DEPLOY_HOOK_URL: ${{ secrets.RENDER_DEPLOY_HOOK_URL }}
                run: |
                # The `curl` command sends a POST request to the Render deploy hook URL
                # to trigger the deployment.
                curl -X POST "$DEPLOY_HOOK_URL"
     ```

4. **Configure Render**:
   - Log in to your Render dashboard and create a new web service.
   - Connect your GitHub repository to Render.
   - Add the following environment variables to Render:
     - `RENDER_SERVICE_ID`: Your Render service ID.
     - `RENDER_API_KEY`: Your Render API key.

5. **Set Up Branch Protection**:
   - Go to your GitHub repository settings.
   - Navigate to "Branches" and add a branch protection rule for the `develop` branch.
   - Require status checks to pass before merging and include the `test` job from the CI/CD workflow.

## Usage

1. **Create a Pull Request to `develop`**:
   - Make changes in a feature branch.
   - Create a pull request to the `develop` branch.
   - The CI/CD pipeline will automatically run component tests.

2. **Merge to `develop`**:
   - If all tests pass, the pull request can be merged into `develop`.

3. **Create a Pull Request to `main`**:
   - Once your changes are in `develop`, create a pull request to the `main` branch.
   - The pipeline will run tests again and automatically merge if they pass.

4. **Auto-Deploy to Render**:
   - After merging into `main`, the application will automatically deploy to Render.

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Submit a pull request to the `develop` branch.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Support

For any issues or questions, please open an issue in the GitHub repository or contact the maintainers.

---

Happy coding! 🚀