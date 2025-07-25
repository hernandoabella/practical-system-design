Designing a Web Crawler (Search Engine Crawler)
A Web Crawler is a distributed system that systematically browses the web, downloads pages, extracts links, and indexes content for search engines, SEO tools, or data aggregation services.

✅ Functional Requirements
Given a set of URLs, crawl them recursively

Extract and store links from each page

Fetch and store HTML content and metadata

Respect robots.txt and rate-limits

Avoid crawling duplicate or forbidden content

Support parallel crawling for scale

Handle failures, timeouts, and retries

🚫 Non-Functional Requirements
High scalability (millions of pages/hour)

Politeness (respect domains and avoid overloading sites)

Distributed, fault-tolerant architecture

Fast and efficient storage of data

Modular parsing and extraction

🧱 Core Components
Component	Role
URL Frontier	Queue of URLs to be crawled, prioritized and deduplicated
Fetcher Service	Downloads pages using HTTP requests
Parser/Extractor	Parses HTML, extracts links, metadata, structured content
Robots.txt Manager	Checks rules for each domain before crawling
Scheduler	Manages crawl frequency, politeness policy, and prioritization
Storage System	Stores raw HTML, extracted content, metadata, crawl logs
Duplicate Checker	Avoids redundant crawling using URL hashing or content checksums
Indexer (Optional)	Sends parsed data to a search indexer (e.g., Elasticsearch)
Monitor & Dashboard	Track performance, status, and logs

🏗️ High-Level Architecture
plaintext
Copiar
Editar
[Seed URLs]
     ↓
[Scheduler] ───→ [URL Frontier Queue] ──→ [Fetcher Service] ──→ [Parser]
     ↑                                                 │
[Politeness Manager] ←──── [Robots.txt Service]        ↓
                   ←──── [Duplicate Checker]     [Content Store]
                                                 [Metadata Store]
                                                 [Index/Search Engine]
🧮 Data Model (Simplified)
sql
Copiar
Editar
PAGES(id, url, domain, crawl_status, fetch_time, content_hash)

RAW_HTML(page_id, html_text, content_type)

LINKS(from_page_id, to_url, anchor_text)

DOMAINS(domain_name, crawl_delay, disallowed_paths, robots_fetched)

CRAWL_LOGS(timestamp, url, status_code, latency)
🔐 Politeness & Compliance
robots.txt parser service — loaded per domain, cached in memory

Per-domain crawl delay enforcement

Per-IP rate-limiting

Blacklist and whitelist URL filters

User-Agent string spoofing should be avoided in ethical crawlers

💡 Link Prioritization Strategies
Priority queue (e.g., Breadth-first or PageRank-based)

Depth limit (e.g., stop after 3 hops)

Custom ranking (e.g., focus on .edu, .gov, or specific languages)

Recent updates via sitemap.xml

⚙️ Fault Tolerance & Retry Strategy
Scenario	Handling Method
HTTP 429 (Too many requests)	Retry with exponential backoff
Connection timeout	Retry up to N times, then log error
Parse errors	Store raw HTML for later inspection
Duplicate content	Skip based on URL hash or body hash

🔁 Scaling Considerations
Layer	Strategy
Fetchers	Horizontal scaling via distributed workers
Frontier Queue	Use Redis/Kafka for distributed queuing
Robots.txt Cache	In-memory LRU cache or Redis
Storage	NoSQL DB (Cassandra), or Blob + Metadata DB
Monitoring	Prometheus + Grafana

🧠 Interview Soundbite
“To build a scalable web crawler, I’d maintain a distributed frontier queue, prioritize URLs, and respect crawl policies using a centralized robots.txt service. Pages would be fetched by workers, parsed to extract links and metadata, and stored efficiently. Duplicate detection would rely on content hashing and Bloom filters. The system is fault-tolerant and respects rate limits per domain.”

📊 Key Technologies
Component	Technology
Queue	Kafka / Redis Streams
Fetcher	Python (Scrapy), Go, Node
HTML Parser	BeautifulSoup, lxml, Cheerio
Storage	S3 + PostgreSQL/Cassandra
Indexing (optional)	Elasticsearch
Monitoring	Prometheus + Grafana