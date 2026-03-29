# Python & FastAPI Guidelines

> **Cách dùng:** Copy file này vào `.clinerules/` khi làm việc với Python/FastAPI project.

## Code Style

- Theo **PEP 8** — dùng Black formatter
- Type hints bắt buộc cho tất cả function signatures
- Docstring cho public functions (Google style):
  ```python
  def calculate_discount(price: float, code: str) -> float:
      """Tính giá sau khi áp dụng mã giảm giá.

      Args:
          price: Giá gốc (VND).
          code: Mã giảm giá.

      Returns:
          Giá sau giảm giá.

      Raises:
          InvalidDiscountError: Nếu mã không hợp lệ.
      """
  ```

## FastAPI Structure

```
src/
├── api/
│   ├── deps.py             # Shared dependencies
│   └── v1/
│       ├── routes/         # Route handlers
│       └── schemas/        # Pydantic models
├── core/
│   ├── config.py           # Settings (pydantic-settings)
│   └── security.py
├── db/
│   ├── models/             # SQLAlchemy models
│   └── repositories/       # DB access layer
└── services/               # Business logic
```

## Request/Response

```python
# ✅ Pydantic schema validation
class CreateOrderRequest(BaseModel):
    items: list[OrderItem]
    discount_code: str | None = None

    @validator('items')
    def items_not_empty(cls, v):
        if not v:
            raise ValueError('items cannot be empty')
        return v

@router.post('/orders', response_model=OrderResponse, status_code=201)
async def create_order(
    body: CreateOrderRequest,
    db: AsyncSession = Depends(get_db),
    current_user: User = Depends(get_current_user),
):
    return await order_service.create(db, body, current_user.id)
```

## Async & Database

- Dùng **async/await** toàn bộ (FastAPI async-first)
- SQLAlchemy async: `AsyncSession`, không dùng sync session
- Repository pattern — tách DB query khỏi business logic
- Alembic cho migrations — không sửa schema trực tiếp

## Settings

```python
# core/config.py
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    DATABASE_URL: str
    SECRET_KEY: str
    DEBUG: bool = False

    class Config:
        env_file = '.env'

settings = Settings()
```

## Không được làm

- Không dùng `*` import: `from module import *`
- Không dùng mutable default args: `def f(items=[])` → `def f(items=None)`
- Không bắt Exception quá rộng mà không re-raise
- Không dùng `print()` để debug — dùng `logging`
