<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Alperen Özdemir - Backend Developer</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
            line-height: 1.6;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem;
        }

        .profile-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 24px;
            box-shadow: 0 25px 50px rgba(0, 0, 0, 0.15);
            overflow: hidden;
            position: relative;
        }

        .profile-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, #667eea, #764ba2, #f093fb, #f5576c);
        }

        .header {
            padding: 3rem 3rem 2rem;
            text-align: center;
            background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
            position: relative;
        }

        .avatar {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            background: linear-gradient(135deg, #667eea, #764ba2);
            margin: 0 auto 1.5rem;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3rem;
            color: white;
            font-weight: bold;
            box-shadow: 0 15px 35px rgba(102, 126, 234, 0.4);
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }

        .name {
            font-size: 2.5rem;
            font-weight: 800;
            margin-bottom: 0.5rem;
            background: linear-gradient(135deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .title {
            font-size: 1.25rem;
            color: #64748b;
            margin-bottom: 1rem;
            font-weight: 500;
        }

        .description {
            font-size: 1.1rem;
            color: #475569;
            max-width: 600px;
            margin: 0 auto;
            font-style: italic;
        }

        .content {
            padding: 2rem 3rem 3rem;
        }

        .section {
            margin-bottom: 3rem;
        }

        .section-title {
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 1.5rem;
            display: flex;
            align-items: center;
            gap: 0.75rem;
            color: #1e293b;
        }

        .section-icon {
            width: 24px;
            height: 24px;
            background: linear-gradient(135deg, #667eea, #764ba2);
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 14px;
        }

        .about-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 1.5rem;
        }

        .about-item {
            background: #f8fafc;
            padding: 1.5rem;
            border-radius: 16px;
            border-left: 4px solid #667eea;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .about-item:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
        }

        .tech-stack {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 1.5rem;
        }

        .tech-category {
            background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
            padding: 1.5rem;
            border-radius: 16px;
            border: 1px solid #e2e8f0;
            transition: all 0.3s ease;
        }

        .tech-category:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            border-color: #667eea;
        }

        .tech-category h3 {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 0.75rem;
            color: #1e293b;
        }

        .tech-tags {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem;
        }

        .tech-tag {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            padding: 0.4rem 0.8rem;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 500;
            transition: transform 0.2s ease;
        }

        .tech-tag:hover {
            transform: scale(1.05);
        }

        .projects-grid {
            display: grid;
            gap: 1.5rem;
        }

        .project-card {
            background: linear-gradient(135deg, #ffffff 0%, #f8fafc 100%);
            border: 1px solid #e2e8f0;
            border-radius: 16px;
            padding: 2rem;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .project-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 3px;
            background: linear-gradient(90deg, #667eea, #764ba2);
            transform: scaleX(0);
            transition: transform 0.3s ease;
        }

        .project-card:hover::before {
            transform: scaleX(1);
        }

        .project-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
            border-color: #667eea;
        }

        .project-header {
            display: flex;
            justify-content: between;
            align-items: flex-start;
            margin-bottom: 1rem;
            gap: 1rem;
        }

        .project-title {
            font-size: 1.25rem;
            font-weight: 700;
            color: #1e293b;
            flex: 1;
        }

        .status-badge {
            padding: 0.3rem 0.8rem;
            border-radius: 20px;
            font-size: 0.75rem;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .status-progress { background: #fef3c7; color: #92400e; }
        .status-architecture { background: #ddd6fe; color: #6b21a8; }
        .status-completed { background: #d1fae5; color: #065f46; }

        .project-stack {
            color: #667eea;
            font-weight: 600;
            margin-bottom: 0.75rem;
        }

        .project-description {
            color: #64748b;
            line-height: 1.6;
        }

        .highlights {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 1.5rem;
        }

        .highlight-card {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 2rem;
            border-radius: 16px;
            text-align: center;
            position: relative;
            overflow: hidden;
            transition: transform 0.3s ease;
        }

        .highlight-card::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(45deg, transparent, rgba(255,255,255,0.1), transparent);
            transform: rotate(45deg);
            transition: all 0.6s ease;
            opacity: 0;
        }

        .highlight-card:hover::before {
            animation: shine 0.6s ease-in-out;
        }

        .highlight-card:hover {
            transform: translateY(-3px);
        }

        @keyframes shine {
            0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); opacity: 0; }
            50% { opacity: 1; }
            100% { transform: translateX(100%) translateY(100%) rotate(45deg); opacity: 0; }
        }

        .highlight-icon {
            font-size: 2rem;
            margin-bottom: 1rem;
        }

        .highlight-title {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 0.5rem;
        }

        .highlight-desc {
            font-size: 0.9rem;
            opacity: 0.9;
        }

        .connect-section {
            text-align: center;
            background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
            padding: 2rem;
            border-radius: 16px;
            margin-top: 2rem;
        }

        .social-links {
            display: flex;
            justify-content: center;
            gap: 1rem;
            margin-top: 1.5rem;
        }

        .social-link {
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            padding: 0.75rem 1.5rem;
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            text-decoration: none;
            border-radius: 12px;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .social-link:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(102, 126, 234, 0.4);
        }

        .quote {
            font-style: italic;
            font-size: 1.1rem;
            color: #475569;
            text-align: center;
            margin-top: 2rem;
            padding: 1.5rem;
            background: linear-gradient(135deg, #f1f5f9 0%, #e2e8f0 100%);
            border-radius: 12px;
            border-left: 4px solid #667eea;
        }

        @media (max-width: 768px) {
            .container { padding: 1rem; }
            .header { padding: 2rem 1.5rem 1.5rem; }
            .content { padding: 1.5rem; }
            .name { font-size: 2rem; }
            .tech-stack { grid-template-columns: 1fr; }
            .highlights { grid-template-columns: 1fr; }
            .social-links { flex-direction: column; align-items: center; }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="profile-card">
            <div class="header">
                <div class="avatar">AÖ</div>
                <h1 class="name">Alperen Özdemir</h1>
                <p class="title">Backend-Focused Full Stack Developer</p>
                <p class="description">Building secure and scalable systems with PHP, Laravel, Django & Docker.</p>
            </div>

            <div class="content">
                <div class="section">
                    <h2 class="section-title">
                        <span class="section-icon">🧠</span>
                        About Me
                    </h2>
                    <div class="about-grid">
                        <div class="about-item">
                            <strong>Backend Architecture Specialist</strong><br>
                            Passionate about clean code, scalable system design, and modern development practices.
                        </div>
                        <div class="about-item">
                            <strong>API & Security Expert</strong><br>
                            Experienced in RESTful API design, containerization, and implementing robust security measures.
                        </div>
                        <div class="about-item">
                            <strong>Performance Optimizer</strong><br>
                            Loves exploring performance tuning, API gateway patterns, and DevSecOps methodologies.
                        </div>
                    </div>
                </div>

                <div class="section">
                    <h2 class="section-title">
                        <span class="section-icon">🛠️</span>
                        Tech Stack
                    </h2>
                    <div class="tech-stack">
                        <div class="tech-category">
                            <h3>Languages</h3>
                            <div class="tech-tags">
                                <span class="tech-tag">PHP</span>
                                <span class="tech-tag">Python</span>
                                <span class="tech-tag">JavaScript</span>
                            </div>
                        </div>
                        <div class="tech-category">
                            <h3>Frameworks</h3>
                            <div class="tech-tags">
                                <span class="tech-tag">Laravel</span>
                                <span class="tech-tag">Django</span>
                                <span class="tech-tag">Symfony</span>
                            </div>
                        </div>
                        <div class="tech-category">
                            <h3>Databases</h3>
                            <div class="tech-tags">
                                <span class="tech-tag">PostgreSQL</span>
                                <span class="tech-tag">MariaDB</span>
                                <span class="tech-tag">Redis</span>
                            </div>
                        </div>
                        <div class="tech-category">
                            <h3>DevOps & Tools</h3>
                            <div class="tech-tags">
                                <span class="tech-tag">Docker</span>
                                <span class="tech-tag">Kubernetes</span>
                                <span class="tech-tag">CI/CD</span>
                                <span class="tech-tag">Linux</span>
                            </div>
                        </div>
                        <div class="tech-category">
                            <h3>Architecture</h3>
                            <div class="tech-tags">
                                <span class="tech-tag">REST API</span>
                                <span class="tech-tag">MVC</span>
                                <span class="tech-tag">Microservices</span>
                            </div>
                        </div>
                        <div class="tech-category">
                            <h3>Security</h3>
                            <div class="tech-tags">
                                <span class="tech-tag">WAF Integration</span>
                                <span class="tech-tag">Access Control</span>
                                <span class="tech-tag">Security Audits</span>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="section">
                    <h2 class="section-title">
                        <span class="section-icon">📂</span>
                        Featured Projects
                    </h2>
                    <div class="projects-grid">
                        <div class="project-card">
                            <div class="project-header">
                                <h3 class="project-title">E-Commerce Platform</h3>
                                <span class="status-badge status-progress">In Progress</span>
                            </div>
                            <div class="project-stack">Laravel + Docker</div>
                            <p class="project-description">
                                Modular microservice setup with advanced cart management, payment integration, and real-time inventory tracking. Built with clean architecture principles and comprehensive API documentation.
                            </p>
                        </div>
                        <div class="project-card">
                            <div class="project-header">
                                <h3 class="project-title">API Gateway</h3>
                                <span class="status-badge status-architecture">Architecture</span>
                            </div>
                            <div class="project-stack">Kubernetes + Nginx</div>
                            <p class="project-description">
                                High-throughput routing layer designed to handle millions of requests with intelligent load balancing, rate limiting, and comprehensive monitoring capabilities.
                            </p>
                        </div>
                        <div class="project-card">
                            <div class="project-header">
                                <h3 class="project-title">Security Scanner</h3>
                                <span class="status-badge status-completed">Completed</span>
                            </div>
                            <div class="project-stack">PHP + Docker</div>
                            <p class="project-description">
                                Automated WAF analysis tool that performs comprehensive security audits, vulnerability assessments, and generates detailed security reports for web applications.
                            </p>
                        </div>
                    </div>
                </div>

                <div class="section">
                    <h2 class="section-title">
                        <span class="section-icon">🏆</span>
                        Key Achievements
                    </h2>
                    <div class="highlights">
                        <div class="highlight-card">
                            <div class="highlight-icon">🚀</div>
                            <div class="highlight-title">Performance Optimization</div>
                            <div class="highlight-desc">Optimized API latency by 60% through Redis caching & advanced query tuning</div>
                        </div>
                        <div class="highlight-card">
                            <div class="highlight-icon">🔒</div>
                            <div class="highlight-title">Security Implementation</div>
                            <div class="highlight-desc">Secured 50+ endpoints with WAF integration & multi-layer token validation</div>
                        </div>
                        <div class="highlight-card">
                            <div class="highlight-icon">🔄</div>
                            <div class="highlight-title">Infrastructure Migration</div>
                            <div class="highlight-desc">Successfully migrated legacy applications to containerized infrastructure</div>
                        </div>
                        <div class="highlight-card">
                            <div class="highlight-icon">⚙️</div>
                            <div class="highlight-title">Scalable Architecture</div>
                            <div class="highlight-desc">Designed robust APIs handling 10M+ daily requests with intelligent load balancing</div>
                        </div>
                    </div>
                </div>

                <div class="connect-section">
                    <h2 class="section-title">
                        <span class="section-icon">🤝</span>
                        Let's Connect
                    </h2>
                    <p style="color: #64748b; margin-bottom: 1rem;">
                        Ready to collaborate on innovative projects and discuss cutting-edge backend solutions.
                    </p>
                    <div class="social-links">
                        <a href="https://www.linkedin.com/in/thealpozdemir" class="social-link">
                            <span>💼</span> LinkedIn
                        </a>
                        <a href="mailtp:alpozzdemir@icloud.com" class="social-link">
                            <span>📧</span> Email
                        </a>
                        <a href="https://github.com/fetchAlp/" class="social-link">
                            <span>💻</span> GitHub
                        </a>
                    </div>
                </div>

                <div class="quote">
                    "Clean code is not written by following rules. It's written by a developer who cares."
                </div>
            </div>
        </div>
    </div>
</body>
</html>
