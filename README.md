# Chirag Bansal - Portfolio Website

A modern, responsive portfolio website built with Jekyll and hosted on GitHub Pages. Features a clean design with orange/purple color scheme and showcases data science projects and insights.

## 🚀 Live Website

Visit the live website at: [https://chiragb254.github.io](https://chiragb254.github.io)

## ✨ Features

- **Responsive Design**: Works perfectly on desktop, tablet, and mobile
- **Dark/Light Theme**: Toggle between light and dark modes
- **Dynamic Metrics**: Real-time GitHub stats and system monitoring
- **Blog System**: Integrated Jekyll blog with categorization and tagging
- **Modern UI**: Clean, professional design with smooth animations
- **SEO Optimized**: Built-in SEO tags and sitemap

## 🛠️ Tech Stack

- **Framework**: Jekyll (GitHub Pages compatible)
- **Styling**: Custom CSS with CSS Variables
- **Icons**: Font Awesome 6
- **Fonts**: Inter (Google Fonts)
- **Hosting**: GitHub Pages
- **Analytics**: GitHub API integration for real-time stats

## 📁 Project Structure

```
chirag-portfolio/
├── _config.yml                 # Jekyll configuration
├── _layouts/                   # Page templates
│   ├── default.html            # Base layout
│   └── post.html              # Blog post layout
├── _posts/                     # Blog posts
│   ├── 2024-12-15-understanding-data-science-lifecycle.md
│   ├── 2024-12-10-feature-engineering-techniques-that-actually-work.md
│   └── 2024-12-05-ab-testing-guide-for-data-scientists.md
├── assets/
│   └── resume/
│       └── chirag-bansal-resume.pdf
├── index.html                  # Homepage
├── posts.html                  # Blog listing page
├── Gemfile                     # Ruby dependencies
└── README.md                   # This file
```

## 🚀 Getting Started

### Prerequisites

- Ruby (2.7 or later)
- Bundler gem
- Git

### Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/chiragb254/chiragb254.github.io.git
   cd chiragb254.github.io
   ```

2. **Install dependencies**
   ```bash
   bundle install
   ```

3. **Run locally**
   ```bash
   bundle exec jekyll serve
   ```

4. **Open in browser**
   Navigate to `http://localhost:4000`

### Making Changes

1. **Update content** in the relevant files
2. **Test locally** using `bundle exec jekyll serve`
3. **Commit and push** to GitHub
4. **GitHub Pages will automatically deploy** the changes

## 📝 Adding Blog Posts

Create new blog posts in the `_posts/` directory with the following format:

```markdown
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD
category: "Category Name"
tags: [tag1, tag2, tag3]
description: "Brief description for SEO and previews"
author: "Chirag Bansal"
---

# Your Post Title

Your content here...
```

**File naming convention**: `YYYY-MM-DD-title-with-hyphens.md`

## 🎨 Customization

### Colors

The color scheme is defined in CSS variables at the top of each file:

```css
:root {
    --accent-color: #ff6b35;        /* Primary orange */
    --secondary-accent: #7c3aed;    /* Purple */
    --bg-color: #ffffff;            /* Background */
    --text-color: #1a1a1a;         /* Text */
    /* ... more colors */
}
```

### Content

- **Personal Info**: Update `_config.yml` with your details
- **Resume**: Replace `assets/resume/chirag-bansal-resume.pdf`
- **Social Links**: Update links in `index.html` and `_config.yml`
- **Professional Summary**: Edit the hero section in `index.html`

## 🔧 Configuration

Key configuration options in `_config.yml`:

```yaml
title: "Chirag Bansal"
description: "Data Scientist specializing in machine learning and analytics solutions"
url: "https://chiragb254.github.io"
author:
  name: "Chirag Bansal"
  email: "chirag.bansal@email.com"
  linkedin: "https://www.linkedin.com/in/chiragb254/"
  github: "https://github.com/chiragb254"
  kaggle: "https://www.kaggle.com/chiragb254"
```

## 📈 Analytics & Monitoring

The website includes:
- **GitHub API Integration**: Real-time commit stats and repository information
- **System Monitoring**: Simulated development environment metrics
- **Performance Tracking**: Animated counters and progress indicators

## 🌐 Deployment

### GitHub Pages (Automatic)

1. Push to the `main` branch of your `username.github.io` repository
2. GitHub Pages will automatically build and deploy
3. Site will be available at `https://username.github.io`

### Manual Deployment

```bash
# Build the site
bundle exec jekyll build

# The _site/ directory contains the built website
# Upload contents to your web server
```

## 🐛 Troubleshooting

### Common Issues

1. **Site not updating**: Check GitHub Pages build status in repository settings
2. **Styling issues**: Ensure CSS is properly linked and cached
3. **Blog posts not showing**: Verify frontmatter format and date
4. **Local development errors**: Run `bundle update` to update gems

### Debug Mode

Run Jekyll with verbose output:
```bash
bundle exec jekyll serve --verbose
```

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📞 Contact

**Chirag Bansal**
- Email: chirag.bansal@email.com
- LinkedIn: [chiragb254](https://www.linkedin.com/in/chiragb254/)
- GitHub: [chiragb254](https://github.com/chiragb254)
- Kaggle: [chiragb254](https://www.kaggle.com/chiragb254)

---

⭐ If you found this helpful, please give it a star on GitHub!