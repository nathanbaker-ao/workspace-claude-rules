# Testing Conventions

Standards for test file locations, naming, and structure across the Fetch AI workspace.

---

## Repository-Specific Conventions

### Platform (Python/Django)

**Framework:** pytest + pytest-django + pytest-asyncio

**Location:** `tests/` directory mirroring source structure

```
platform/
├── app/
│   └── services/
│       └── feature_service.py
└── tests/
    └── services/
        └── test_feature_service.py
```

**Naming:**
- Files: `test_<module>.py`
- Classes: `Test<ClassName>`
- Functions: `test_<action>_<scenario>`

### Clients (TypeScript/React)

**Framework:** Cypress (component + e2e)

**Location:** `cypress/` directory in fetch-llm

```
apps/fetch-llm/
├── src/
│   └── components/
│       └── feature/
│           └── component.tsx
└── cypress/
    ├── component/
    │   └── feature/
    │       └── component.cy.tsx
    └── e2e/
        └── feature.cy.ts
```

**Naming:**
- Component tests: `<component-name>.cy.tsx`
- E2E tests: `<feature>.cy.ts`

---

## Test Structure

### Platform Test Structure

```python
import pytest
from app.services.feature_service import FeatureService

class TestFeatureService:
    """Tests for FeatureService."""

    # Fixtures at class level if shared
    @pytest.fixture
    def service(self):
        return FeatureService()

    # Happy path tests first
    @pytest.mark.django_db
    def test_creates_feature_successfully(self, service, user):
        """Feature is created with valid input."""
        # Arrange
        data = {"name": "Test", "description": "Desc"}

        # Act
        result = service.create(user=user, **data)

        # Assert
        assert result.id is not None
        assert result.name == "Test"

    # Edge cases and error conditions
    @pytest.mark.django_db
    def test_raises_on_empty_name(self, service, user):
        """ValidationError raised when name is empty."""
        with pytest.raises(ValidationError) as exc_info:
            service.create(user=user, name="")

        assert "name" in str(exc_info.value)

    # Async tests
    @pytest.mark.asyncio
    async def test_async_operation(self, service):
        """Async operation completes successfully."""
        result = await service.async_method()
        assert result is not None
```

### Clients Test Structure

```typescript
// cypress/component/feature/component.cy.tsx
import { mount } from 'cypress/react18';
import { FeatureComponent } from '@/components/feature/component';
import { mockData, mockEmptyData } from '../../support/fixtures';

describe('FeatureComponent', () => {
  // Happy path
  describe('when data is provided', () => {
    it('renders content correctly', () => {
      mount(<FeatureComponent data={mockData} />);

      cy.contains('Expected Title').should('be.visible');
      cy.get('[data-testid="content"]').should('exist');
    });

    it('handles user interaction', () => {
      const onAction = cy.stub().as('onAction');
      mount(<FeatureComponent data={mockData} onAction={onAction} />);

      cy.get('[data-testid="action-button"]').click();
      cy.get('@onAction').should('have.been.calledOnce');
    });
  });

  // Edge cases
  describe('when data is empty', () => {
    it('shows empty state', () => {
      mount(<FeatureComponent data={mockEmptyData} />);

      cy.contains('No data available').should('be.visible');
    });
  });

  // Error states
  describe('when error occurs', () => {
    it('displays error message', () => {
      mount(<FeatureComponent data={mockData} error="Something went wrong" />);

      cy.contains('Something went wrong').should('be.visible');
    });
  });
});
```

---

## Test Naming Conventions

### Descriptive Names

```python
# Good - describes behavior
def test_returns_401_when_token_missing():
    """Returns 401 Unauthorized when auth token is not provided."""

def test_creates_channel_with_valid_participants():
    """Channel is created when all participants are valid users."""

# Bad - vague or implementation-focused
def test_create():
    """Test create."""

def test_channel_service():
    """Test channel service."""
```

### Pattern: `test_<action>_when_<condition>`

```python
# Actions
test_creates_...
test_returns_...
test_raises_...
test_updates_...
test_deletes_...

# Conditions
..._when_valid_input
..._when_unauthorized
..._when_not_found
..._when_duplicate_exists
..._with_empty_list
```

---

## Arrange-Act-Assert Pattern

### Platform

```python
def test_processes_message_correctly(self, service, channel):
    # Arrange - Set up test data and dependencies
    message = MessageFactory(channel=channel, content="Hello")

    # Act - Perform the action being tested
    result = service.process(message)

    # Assert - Verify the outcome
    assert result.processed is True
    assert result.tokens_used > 0
```

### Clients

```typescript
it('submits form with valid data', () => {
  // Arrange
  const onSubmit = cy.stub().as('onSubmit');
  mount(<FormComponent onSubmit={onSubmit} />);

  // Act
  cy.get('[data-testid="name-input"]').type('Test Name');
  cy.get('[data-testid="submit-button"]').click();

  // Assert
  cy.get('@onSubmit').should('have.been.calledWith', { name: 'Test Name' });
});
```

---

## Test Data

### Platform Factories

Use factory_boy for test data:

```python
# tests/factories.py
import factory
from app.models import User, Channel

class UserFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = User

    email = factory.Faker('email')
    name = factory.Faker('name')

class ChannelFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = Channel

    name = factory.Faker('company')
    owner = factory.SubFactory(UserFactory)
```

### Clients Fixtures

```typescript
// cypress/support/fixtures.ts
export const mockUser = {
  id: 'user-123',
  name: 'Test User',
  email: 'test@example.com',
};

export const mockChannel = {
  id: 'channel-456',
  name: 'Test Channel',
  participants: [mockUser],
};

export const mockMessage = {
  id: 'msg-789',
  content: 'Hello world',
  sender: mockUser,
  timestamp: new Date().toISOString(),
};
```

---

## Test Commands

### Platform

```bash
# Run all tests
pytest

# Run specific file
pytest tests/services/test_feature_service.py

# Run specific test
pytest tests/services/test_feature_service.py::TestFeatureService::test_creates_feature

# Run with coverage
pytest --cov=app --cov-report=html

# Run only marked tests
pytest -m "not slow"

# Verbose output
pytest -v --tb=short
```

### Clients

```bash
# Component tests - all
pnpm --filter fetch-llm cypress run --component

# Component tests - specific
pnpm --filter fetch-llm cypress run --component --spec "cypress/component/feature/**"

# E2E tests
pnpm --filter fetch-llm cypress run --e2e

# Interactive mode
pnpm --filter fetch-llm cypress open
```

---

## What to Test

### Test This

- Business logic and calculations
- State transitions
- Error handling
- Edge cases from requirements
- API response formats
- Component rendering states
- User interactions

### Don't Test This

- Framework internals
- Third-party library behavior
- Trivial getters/setters
- Implementation details
- Private methods directly

---

## Test Isolation

### Platform

```python
@pytest.mark.django_db
class TestChannelService:
    """Each test gets fresh database state."""

    @pytest.fixture(autouse=True)
    def setup(self, db):
        # Runs before each test
        self.user = UserFactory()
        self.service = ChannelService()

    def test_one(self):
        # Fresh state
        pass

    def test_two(self):
        # Also fresh state
        pass
```

### Clients

```typescript
describe('Component', () => {
  beforeEach(() => {
    // Reset state before each test
    cy.clearLocalStorage();
  });

  it('test one', () => {
    // Fresh mount
    mount(<Component />);
  });

  it('test two', () => {
    // Also fresh mount
    mount(<Component />);
  });
});
```

---

## Mocking Guidelines

### Platform

```python
from unittest.mock import Mock, patch

def test_sends_notification(self, service):
    """Notification is sent when feature completes."""
    # Mock external service
    with patch('app.services.notification_service.send') as mock_send:
        mock_send.return_value = True

        service.complete_feature()

        mock_send.assert_called_once_with(
            type='feature_complete',
            user_id=self.user.id
        )
```

### Clients

```typescript
it('calls API on submit', () => {
  // Intercept network request
  cy.intercept('POST', '/api/feature', {
    statusCode: 200,
    body: { success: true },
  }).as('createFeature');

  mount(<FeatureForm />);
  cy.get('[data-testid="submit"]').click();

  cy.wait('@createFeature');
  cy.contains('Success').should('be.visible');
});
```

---

## Integration

- TDD workflow uses these conventions: tdd-workflow.md
- Session log records test progress: session-logging.md
- PRs should mention test coverage: github-flow.md
