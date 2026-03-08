# Contributing to if-me

First, thank you for taking the time to contribute! 🎉 We appreciate your interest in helping make if-me a better platform for mental health support.

We use the [Contributor Covenant](http://contributor-covenant.org) as our Code of Conduct. Please [read it](code_of_conduct.md) before joining our project. All contributors are expected to uphold this code.

## Table of Contents

- [Getting Started](#getting-started)
- [Ways to Contribute](#ways-to-contribute)
- [Development Setup](#development-setup)
- [Making Changes](#making-changes)
- [Submitting Changes](#submitting-changes)
- [Coding Standards](#coding-standards)
- [Testing](#testing)
- [Commit Messages](#commit-messages)
- [Pull Request Process](#pull-request-process)
- [Community](#community)
- [Need Help?](#need-help)

## Getting Started

1. **Read the Code of Conduct** - Ensure you understand our values and community standards
2. **Explore the Project** - Check out the [Wiki](https://github.com/ifmeorg/ifme/wiki) for full documentation
3. **Join Our Community** - [Join our Slack workspace](https://github.com/ifmeorg/ifme/wiki/Join-Our-Slack) to connect with other contributors
4. **Check Current Issues** - Look at [open issues](https://github.com/Byrnesz/ifme/issues) to find tasks you can help with

## Ways to Contribute

### 👨‍💻 Developers
- Report bugs and suggest features
- Write and improve code
- Improve error handling and performance
- Write tests and documentation
- Review pull requests

### 🎨 Designers
- Improve user interface and user experience
- Create mockups and prototypes
- Conduct usability testing
- Design new features

### ✍️ Writers
- Write and improve documentation
- Create tutorials and guides
- Improve copy throughout the app
- Translate content

### 🧪 Testers
- Test features and report bugs
- Conduct quality assurance
- Test across different browsers and devices
- Provide user feedback

### 🌍 Translators
- Translate the application into new languages
- Improve existing translations
- Localize content for different regions

## Development Setup

### Prerequisites

- Ruby 3.0 or higher
- Node.js 16.x or higher
- PostgreSQL 12 or higher (or MySQL 5.7+)
- Git
- Foreman (for running multiple processes)

### Installation

1. **Fork the Repository**
   ```bash
   # Click the "Fork" button on GitHub
   ```

2. **Clone Your Fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/ifme.git
   cd ifme
   ```

3. **Add Upstream Remote**
   ```bash
   git remote add upstream https://github.com/ifmeorg/ifme.git
   ```

4. **Run Setup Script**
   ```bash
   ./bin/start_app
   ```
   This will:
   - Install Ruby gems
   - Install Node packages
   - Start the development server

5. **Access the Application**
   ```
   http://localhost:3000
   ```

### Manual Setup (if bin/start_app doesn't work)

```bash
# Install backend dependencies
bundle install

# Install frontend dependencies
cd client && yarn install && cd ..

# Create and migrate database
bundle exec rails db:create
bundle exec rails db:migrate

# Start development server
foreman start -f Procfile.dev

# In another terminal, start Sidekiq (for background jobs)
bundle exec sidekiq
```

## Making Changes

### Before You Start

1. **Check for Existing Issues** - Search [issues](https://github.com/Byrnesz/ifme/issues) to avoid duplicate work
2. **Discuss Major Changes** - For significant changes, open an issue first to discuss
3. **Keep Issues Updated** - Comment on issues you're working on to avoid conflicts

### Create a Feature Branch

```bash
# Update master branch
git checkout master
git pull upstream master

# Create feature branch
git checkout -b feature/your-feature-name
```

### Branch Naming Convention

Use descriptive names:
- `feature/add-story-drafts`
- `fix/login-validation-bug`
- `docs/improve-readme`
- `refactor/user-model-cleanup`
- `test/add-comment-tests`

## Submitting Changes

### Writing Code

1. **Follow Code Style** - See [Coding Standards](#coding-standards) below
2. **Write Tests** - Add tests for new features or bug fixes
3. **Update Documentation** - Update relevant docs and comments
4. **Avoid Debugging Code** - Remove console.log, debugger statements, etc.

### Keep Your Branch Updated

```bash
# Fetch upstream changes
git fetch upstream

# Rebase your branch
git rebase upstream/master
```

### Commit Your Changes

See [Commit Messages](#commit-messages) section for guidelines.

```bash
git add .
git commit -m "feat(feature): add new feature description"
```

### Push to Your Fork

```bash
git push origin feature/your-feature-name
```

## Coding Standards

### Backend (Ruby/Rails)

**Style Guide**: [Ruby Style Guide](https://rubystyle.guide/)

**Linting**:
```bash
# Run Rubocop
bundle exec rubocop

# Auto-fix issues
bundle exec rubocop -a
```

**Best Practices**:
- Use meaningful variable names
- Keep methods small and focused (under 20 lines)
- Add schema annotations to models
- Use concerns for shared functionality
- Follow Rails conventions
- Handle errors gracefully

**Example**:
```ruby
# frozen_string_literal: true
# == Schema Information
#
# Table name: users
#  id                 :bigint           not null, primary key
#  email              :string           default(""), not null
#

class User < ApplicationRecord
  # Associations
  has_many :stories, dependent: :destroy

  # Validations
  validates :email, presence: true, uniqueness: true

  # Methods
  def display_name
    name.presence || email
  end
end
```

### Frontend (JavaScript/React)

**Linting**:
```bash
# Run ESLint
cd client && yarn lint

# Auto-fix issues
cd client && yarn lint --fix
```

**Type Safety**: We use Flow for type checking
```javascript
// @flow
import React from 'react';

export type Props = {
  id: string,
  title: string,
  onSubmit: (data: any) => void,
};

const MyComponent = ({ id, title, onSubmit }: Props) => {
  return <div>{title}</div>;
};

export default MyComponent;
```

**Best Practices**:
- Use functional components with hooks
- Keep components small and reusable
- Use descriptive prop names
- Add Flow types for all components
- Use CSS modules for styling
- Avoid inline styles

### Both

- Write clear, descriptive comments
- Use meaningful naming conventions
- Keep functions focused on a single responsibility
- Don't repeat code (DRY principle)
- Document complex logic

## Testing

### Backend Tests

**Run All Tests**:
```bash
bundle exec rspec
```

**Run Specific Test File**:
```bash
bundle exec rspec spec/models/user_spec.rb
```

**Run with Coverage**:
```bash
COVERAGE=true bundle exec rspec
```

**Example Test**:
```ruby
RSpec.describe User, type: :model do
  describe '#display_name' do
    context 'when name is present' do
      it 'returns the name' do
        user = build(:user, name: 'John Doe')
        expect(user.display_name).to eq('John Doe')
      end
    end

    context 'when name is blank' do
      it 'returns the email' do
        user = build(:user, name: '', email: 'john@example.com')
        expect(user.display_name).to eq('john@example.com')
      end
    end
  end
end
```

### Frontend Tests

**Run All Tests**:
```bash
cd client && yarn test
```

**Run with Coverage**:
```bash
cd client && yarn test --coverage
```

### Coverage Requirements

- Minimum **80%** overall coverage
- **85%** for critical paths (auth, payments, etc.)
- **100%** for new utility functions

## Commit Messages

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification.

### Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type

- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation changes
- **style**: Code style changes (formatting, semicolons, etc.)
- **refactor**: Code refactoring without feature or bug fix
- **perf**: Performance improvements
- **test**: Test additions or changes
- **chore**: Build, dependencies, or other maintenance

### Scope

- `auth` - Authentication related
- `story` - Story features
- `group` - Group features
- `comment` - Comment features
- `user` - User profile and settings
- `ui` - User interface components
- `api` - API endpoints
- `db` - Database related
- `ci` - CI/CD configuration

### Subject

- Use imperative mood ("add" not "adds" or "added")
- Don't capitalize first letter
- No period (.) at end
- Limit to 50 characters

### Body (Optional)

- Wrap at 72 characters
- Explain **what** and **why**, not how
- Separate from subject with blank line

### Footer (Optional)

- Reference issues: `Closes #123`
- Related issues: `Related-To: #456`

### Examples

**Good**:
```
feat(story): add story draft functionality

Users can now save stories as drafts before publishing.
Drafts are stored in a separate table and accessible
from the user's profile.

Closes #42
```

**Good**:
```
fix(auth): resolve OAuth token expiration issue

Google OAuth tokens were not being refreshed automatically,
causing unexpected logouts. Implemented automatic token
refresh 5 minutes before expiry.

Closes #156
Related-To: #123
```

## Pull Request Process

### Before Submitting

1. **Sync with Upstream**
   ```bash
   git fetch upstream
   git rebase upstream/master
   ```

2. **Run Tests Locally**
   ```bash
   bundle exec rspec
   cd client && yarn test
   ```

3. **Run Linters**
   ```bash
   bundle exec rubocop
   cd client && yarn lint
   ```

### Creating the Pull Request

1. **Push to Your Fork**
   ```bash
   git push origin feature/your-feature-name
   ```

2. **Open Pull Request on GitHub**
   - Use the pull request template
   - Reference related issues
   - Provide clear description of changes
   - Include screenshots for UI changes

3. **PR Title Format**
   ```
   <type>: <description>
   
   Example: feat: add story draft functionality
   ```

### PR Description Template

```markdown
## Description
Brief description of what this PR does.

## Type of Change
- [ ] Bug fix (non-breaking change that fixes an issue)
- [ ] New feature (non-breaking change that adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation update

## Related Issues
Closes #(issue number)

## Testing
- [ ] I have tested this locally
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] All tests pass

## Checklist
- [ ] My code follows the style guidelines
- [ ] I have performed a self-review
- [ ] I have commented my code for complex logic
- [ ] I have updated relevant documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests for my changes
- [ ] New and existing unit tests pass locally

## Screenshots (if applicable)
Add screenshots for UI changes.
```

### Code Review

- Address feedback promptly
- Discuss disagreements respectfully
- Request re-review after making changes
- Be patient - maintainers are volunteers

### Merging

- Your PR will be merged once:
  - All checks pass
  - Code review is approved
  - Tests are passing
  - No conflicts with main branch

## Community

We've formalized processes for different contributor roles. See our [Wiki](https://github.com/ifmeorg/ifme/wiki) for detailed information:

### [👨‍💻 Developer](https://github.com/ifmeorg/ifme/wiki/Developers)
Write and improve code, submit features and bug fixes.

### [🎨 Designer](https://github.com/ifmeorg/ifme/wiki/Designers)
Improve UI/UX, create mockups, conduct usability testing.

### [🧪 Tester](https://github.com/ifmeorg/ifme/wiki/Testers)
Test features, report bugs, conduct QA.

### [✍️ Writer](https://github.com/ifmeorg/ifme/wiki/Writers)
Improve documentation, create guides, improve copy.

### [🌍 Translator](https://github.com/ifmeorg/ifme/wiki/Translations)
Translate the application into new languages.

## Need Help?

- **Join Slack**: [Join our Slack workspace](https://github.com/ifmeorg/ifme/wiki/Join-Our-Slack)
- **Create an Issue**: [Open a discussion issue](https://github.com/Byrnesz/ifme/issues/new)
- **Check Wiki**: [Our comprehensive Wiki](https://github.com/ifmeorg/ifme/wiki)
- **Review Guidelines**: [Contributor guidelines](https://github.com/ifmeorg/ifme/wiki)

## Additional Resources

- [Code of Conduct](code_of_conduct.md)
- [Project Wiki](https://github.com/ifmeorg/ifme/wiki)
- [Issue Tracker](https://github.com/Byrnesz/ifme/issues)
- [Pull Requests](https://github.com/Byrnesz/ifme/pulls)
- [Design System](http://design.if-me.org/)

## Recognition

All contributors are recognized in our [Contributors](https://github.com/ifmeorg/ifme/wiki/Contributor-Blurb) page. We celebrate and value your contributions!

---

**Thank you for contributing to if-me! Together, we're making mental health support more accessible. 💙**
