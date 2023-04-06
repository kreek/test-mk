# Currency & Money

!!! success "Guidance"
    - All monetary amounts **should** be displayed in United States Dollars (USD).
    - Values **should** include the dollar symbol to the left of the amount, with no space.
    - Cents **should** be displayed using decimals and a fractional separator.
    - Amounts less than a dollar **should** start with a zero followed by a decimal.
    - Thousands **should** be separated by commas.

Return monetary values as strings prefixed by a US dollar symbol.

```json title="One thousand dollars and ten cents"
{
  "paymentAmount": "$1,000.10"
}
```

Exact dollar values should still display cents.

```json title="One thousand dollars"
{
  "paymentAmount": "$1,000.00"
}
```

Values less than one dollar are returned in relation to dollars.

```json title="Ten cents"
{
  "paymentAmount": "$0.10"
}
```
