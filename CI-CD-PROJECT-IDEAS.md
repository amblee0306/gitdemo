# Data Pipeline with CI/CD - Detailed Project Plan

## Project Overview

**Difficulty**: ⭐⭐⭐ (Medium)  
**Time**: 3-4 hours  
**CI/CD Features**: Data validation, pipeline testing, scheduled deployments, monitoring

### Description
An ETL (Extract, Transform, Load) data pipeline built with Python that demonstrates CI/CD best practices for data engineering. The pipeline will process data from a source, validate it, transform it, and load it into a destination, all with automated testing and deployment.

---

## Learning Objectives

By completing this project, you will learn:
- ✅ Data pipeline testing strategies
- ✅ Data validation in CI pipelines
- ✅ Docker containerization for data pipelines
- ✅ Scheduled job deployment automation
- ✅ Data quality checks and monitoring
- ✅ Environment-specific configurations
- ✅ Error handling and alerting
- ✅ Version control for data pipelines

---

## Tech Stack

### Core Technologies
- **Python 3.9+**: Primary programming language
- **Pandas**: Data manipulation and transformation
- **Pytest**: Testing framework
- **Great Expectations** or **Pandera**: Data validation
- **Docker**: Containerization
- **GitHub Actions** or **GitLab CI**: CI/CD platform

### Optional/Advanced
- **Prefect** or **Apache Airflow**: Workflow orchestration (for advanced version)
- **PostgreSQL** or **SQLite**: Destination database
- **CSV/JSON files**: Source data (for simplicity)
- **AWS S3** or **Google Cloud Storage**: Cloud storage (optional)

---

## Project Structure

```
data-pipeline/
├── .github/
│   └── workflows/
│       ├── ci.yml              # Continuous Integration
│       └── deploy.yml           # Deployment workflow
├── src/
│   ├── __init__.py
│   ├── extract.py              # Data extraction logic
│   ├── transform.py            # Data transformation logic
│   ├── load.py                 # Data loading logic
│   ├── pipeline.py             # Main pipeline orchestrator
│   └── validators.py           # Data validation functions
├── tests/
│   ├── __init__.py
│   ├── test_extract.py
│   ├── test_transform.py
│   ├── test_load.py
│   ├── test_pipeline.py
│   ├── test_validators.py
│   └── fixtures/
│       └── sample_data.csv     # Test data
├── data/
│   ├── raw/                    # Raw input data
│   ├── processed/              # Processed output data
│   └── .gitkeep
├── docker/
│   └── Dockerfile
├── .gitignore
├── requirements.txt
├── requirements-dev.txt
├── pytest.ini
├── README.md
└── docker-compose.yml          # For local testing
```

---

## Detailed Implementation Plan

### Phase 1: Project Setup (30 minutes)

#### 1.1 Initialize Project
- [ ] Create project directory structure
- [ ] Initialize Git repository
- [ ] Create virtual environment
- [ ] Set up Python package structure

#### 1.2 Dependencies
- [ ] Create `requirements.txt` with:
  - pandas
  - pytest
  - great-expectations (or pandera)
  - python-dotenv
- [ ] Create `requirements-dev.txt` with:
  - black (code formatter)
  - flake8 (linter)
  - pytest-cov (coverage)
  - mypy (type checking)

#### 1.3 Configuration Files
- [ ] Create `.gitignore` for Python, data files, and IDE
- [ ] Create `pytest.ini` for test configuration
- [ ] Create `.env.example` for environment variables

---

### Phase 2: Pipeline Development (1.5 hours)

#### 2.1 Data Extraction Module (`src/extract.py`)
**Purpose**: Extract data from source

**Features**:
- Read data from CSV/JSON file or database
- Handle different file formats
- Error handling for missing files
- Logging for extraction process

**Example Implementation**:
```python
def extract_data(source_path: str) -> pd.DataFrame:
    """
    Extract data from source.
    
    Args:
        source_path: Path to source data file
        
    Returns:
        DataFrame with raw data
        
    Raises:
        FileNotFoundError: If source file doesn't exist
    """
    # Implementation here
    pass
```

#### 2.2 Data Validation Module (`src/validators.py`)
**Purpose**: Validate data quality

**Features**:
- Schema validation (column names, types)
- Data quality checks (nulls, duplicates, ranges)
- Business rule validation
- Generate validation reports

**Validation Checks**:
- Required columns exist
- Data types are correct
- No null values in critical columns
- Values within expected ranges
- No duplicate records (if applicable)

#### 2.3 Data Transformation Module (`src/transform.py`)
**Purpose**: Transform raw data into processed format

**Features**:
- Data cleaning (remove nulls, duplicates)
- Data type conversions
- Column renaming/selection
- Aggregations or calculations
- Data enrichment

**Example Transformations**:
- Clean and standardize dates
- Convert currency formats
- Calculate derived metrics
- Filter invalid records

#### 2.4 Data Loading Module (`src/load.py`)
**Purpose**: Load processed data to destination

**Features**:
- Write to database or file
- Handle upserts/updates
- Transaction management
- Error handling and rollback

**Destinations**:
- CSV file (for simplicity)
- SQLite database (for local)
- PostgreSQL (for production)

#### 2.5 Pipeline Orchestrator (`src/pipeline.py`)
**Purpose**: Coordinate the entire ETL process

**Features**:
- Run extract → validate → transform → load
- Error handling and recovery
- Logging and monitoring
- Configuration management

---

### Phase 3: Testing (1 hour)

#### 3.1 Unit Tests
- [ ] Test extraction with various file formats
- [ ] Test validation logic with valid/invalid data
- [ ] Test transformation functions
- [ ] Test loading to different destinations
- [ ] Test error handling

#### 3.2 Integration Tests
- [ ] Test full pipeline with sample data
- [ ] Test pipeline with invalid data (should fail gracefully)
- [ ] Test data validation failures
- [ ] Test end-to-end flow

#### 3.3 Data Quality Tests
- [ ] Test schema validation
- [ ] Test data quality rules
- [ ] Test edge cases (empty files, malformed data)

#### 3.4 Test Coverage
- [ ] Aim for 80%+ code coverage
- [ ] Use `pytest-cov` to generate reports
- [ ] Include coverage in CI pipeline

---

### Phase 4: Docker Setup (30 minutes)

#### 4.1 Dockerfile
- [ ] Create multi-stage Dockerfile
- [ ] Optimize for size and security
- [ ] Set up proper working directory
- [ ] Configure entrypoint

#### 4.2 Docker Compose (for local testing)
- [ ] Set up local database (if using)
- [ ] Configure volumes for data
- [ ] Set environment variables

---

### Phase 5: CI/CD Pipeline (1 hour)

#### 5.1 Continuous Integration (`.github/workflows/ci.yml`)

**Pipeline Steps**:

1. **Checkout Code**
   ```yaml
   - uses: actions/checkout@v3
   ```

2. **Set Up Python**
   ```yaml
   - uses: actions/setup-python@v4
     with:
       python-version: '3.9'
   ```

3. **Install Dependencies**
   ```yaml
   - run: pip install -r requirements.txt -r requirements-dev.txt
   ```

4. **Code Quality Checks**
   ```yaml
   - run: flake8 src/ tests/
   - run: black --check src/ tests/
   - run: mypy src/
   ```

5. **Run Tests**
   ```yaml
   - run: pytest tests/ --cov=src --cov-report=xml --cov-report=html
   ```

6. **Check Coverage Threshold**
   ```yaml
   - run: pytest --cov=src --cov-fail-under=80
   ```

7. **Data Validation Tests**
   ```yaml
   - run: pytest tests/test_validators.py -v
   ```

8. **Upload Coverage Reports**
   ```yaml
   - uses: codecov/codecov-action@v3
     with:
       files: ./coverage.xml
   ```

**Triggers**:
- On push to any branch
- On pull requests
- On schedule (daily/weekly)

#### 5.2 Deployment Pipeline (`.github/workflows/deploy.yml`)

**Pipeline Steps**:

1. **Build Docker Image**
   ```yaml
   - name: Build Docker image
     run: docker build -t data-pipeline:${{ github.sha }} .
   ```

2. **Run Pipeline Tests in Container**
   ```yaml
   - name: Test pipeline in container
     run: docker run data-pipeline:${{ github.sha }} pytest tests/
   ```

3. **Deploy to Staging** (on merge to `develop`)
   - Tag image as `staging`
   - Push to container registry
   - Deploy to staging environment
   - Run smoke tests with sample data

4. **Deploy to Production** (on merge to `main`)
   - Tag image as `production` or version number
   - Push to container registry
   - Deploy to production
   - Run validation checks
   - Set up monitoring alerts

**Environment Variables**:
- Database connection strings
- API keys (stored as secrets)
- Data source paths
- Destination configurations

---

### Phase 6: Advanced Features (Optional)

#### 6.1 Scheduling
- [ ] Set up cron job or scheduled workflow
- [ ] Configure daily/weekly pipeline runs
- [ ] Handle timezone considerations

#### 6.2 Monitoring & Alerting
- [ ] Add logging to all modules
- [ ] Set up error notifications (email/Slack)
- [ ] Create monitoring dashboard
- [ ] Track pipeline execution metrics

#### 6.3 Data Quality Dashboard
- [ ] Generate data quality reports
- [ ] Track validation failures over time
- [ ] Create visualization of data quality metrics

#### 6.4 Version Control for Data
- [ ] Implement data versioning
- [ ] Track data lineage
- [ ] Store data snapshots

---

## Sample Data Pipeline Use Case

### Example: Sales Data Processing Pipeline

**Source**: CSV file with sales transactions
**Destination**: PostgreSQL database with aggregated sales data

**Pipeline Steps**:
1. **Extract**: Read sales CSV file
2. **Validate**: 
   - Check required columns (date, product, amount, customer_id)
   - Validate data types
   - Check for null values
   - Verify amount > 0
3. **Transform**:
   - Standardize date format
   - Calculate total sales per product
   - Add derived columns (month, quarter)
   - Remove duplicates
4. **Load**: 
   - Insert into `sales_summary` table
   - Update existing records if needed

---

## CI/CD Pipeline Flow Diagram

```
┌─────────────────┐
│  Code Push/PR   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Install Deps   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Lint & Format  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Run Unit Tests │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Data Validation│
│     Tests       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Check Coverage  │
│   (>= 80%)      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Build Docker   │
│     Image       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Deploy Staging │
│  (if develop)   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Deploy Prod     │
│  (if main)      │
└─────────────────┘
```

---

## Success Criteria

✅ All tests pass in CI pipeline  
✅ Code coverage >= 80%  
✅ Data validation catches invalid data  
✅ Pipeline runs successfully in Docker container  
✅ Deployment to staging works automatically  
✅ Deployment to production requires approval  
✅ Pipeline handles errors gracefully  
✅ Logging provides clear visibility  

---

## Next Steps

1. **Choose CI/CD Platform**: GitHub Actions (recommended) or GitLab CI
2. **Set up Repository**: Create GitHub/GitLab repository
3. **Follow Implementation Plan**: Work through phases 1-5
4. **Test Locally**: Ensure everything works before pushing
5. **Iterate**: Add more features based on learning

---

## Resources

- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [Pytest Documentation](https://docs.pytest.org/)
- [Great Expectations](https://greatexpectations.io/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)

---

## Troubleshooting Tips

- **Tests failing in CI but passing locally**: Check Python version and dependencies
- **Docker build failing**: Verify Dockerfile syntax and base image
- **Data validation too strict**: Adjust validation rules based on real data
- **Coverage below threshold**: Add more test cases for edge cases
- **Deployment failing**: Check environment variables and secrets

---

Ready to start building? Let's create the project structure and begin implementation! 🚀
