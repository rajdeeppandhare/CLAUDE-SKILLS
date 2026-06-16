# App Components — Curated Patterns

Compositions for SaaS dashboards, admin panels, app shells, and data-heavy UI.
Adapt to the user's real data model and brand — never ship placeholder labels.

---

## App shell (sidebar + topbar + content)

**Pattern: fixed sidebar left, sticky topbar, scrollable main**

```jsx
<div style={{ display: 'flex', height: '100vh', background: 'var(--bg-base)' }}>

  {/* Sidebar */}
  <aside style={{
    width: 240, flexShrink: 0,
    borderRight: '1px solid var(--border)',
    display: 'flex', flexDirection: 'column',
    padding: 'var(--space-2)',
  }}>
    {/* Logo */}
    <div style={{ padding: 'var(--space-2)', fontWeight: 700, fontSize: 16, marginBottom: 'var(--space-2)' }}>
      BrandName
    </div>

    {/* Nav items */}
    <nav style={{ flex: 1, display: 'flex', flexDirection: 'column', gap: 2 }}>
      {navItems.map(item => (
        <NavItem key={item.label} icon={item.icon} label={item.label} active={item.active} />
      ))}
    </nav>

    {/* User footer */}
    <div style={{ borderTop: '1px solid var(--border)', paddingTop: 'var(--space-2)', marginTop: 'var(--space-2)' }}>
      <div style={{ display: 'flex', alignItems: 'center', gap: 'var(--space-1)', padding: 'var(--space-1)' }}>
        <Avatar style={{ width: 32, height: 32 }}>
          <AvatarFallback>JS</AvatarFallback>
        </Avatar>
        <div>
          <div style={{ fontSize: 13, fontWeight: 500 }}>Jane Smith</div>
          <div style={{ fontSize: 12, color: 'var(--text-tertiary)' }}>jane@co.com</div>
        </div>
      </div>
    </div>
  </aside>

  {/* Main */}
  <div style={{ flex: 1, display: 'flex', flexDirection: 'column', overflow: 'hidden' }}>
    {/* Topbar */}
    <header style={{
      height: 56, borderBottom: '1px solid var(--border)',
      display: 'flex', alignItems: 'center', justifyContent: 'space-between',
      padding: '0 var(--space-4)', flexShrink: 0,
    }}>
      <h1 style={{ fontSize: 16, fontWeight: 600 }}>Dashboard</h1>
      <div style={{ display: 'flex', gap: 'var(--space-1)', alignItems: 'center' }}>
        <Button variant="ghost" size="icon"><Bell size={16} /></Button>
        <Button size="sm">New report</Button>
      </div>
    </header>

    {/* Content */}
    <main style={{ flex: 1, overflowY: 'auto', padding: 'var(--space-4)' }}>
      {/* page content here */}
    </main>
  </div>
</div>
```

**Nav item sub-component:**
```jsx
const NavItem = ({ icon, label, active }) => (
  <button style={{
    display: 'flex', alignItems: 'center', gap: 'var(--space-1)',
    padding: '6px 10px', borderRadius: 'var(--radius-md)',
    fontSize: 14, fontWeight: active ? 500 : 400,
    background: active ? 'var(--bg-muted)' : 'transparent',
    color: active ? 'var(--text-primary)' : 'var(--text-secondary)',
    border: 'none', cursor: 'pointer', width: '100%', textAlign: 'left',
  }}>
    {icon} {label}
  </button>
)
```

---

## Stat cards

**Pattern: 4-up grid, metric + label + delta**

```jsx
const stats = [
  { label: 'Total revenue', value: '$48,295', delta: '+12.5%', positive: true },
  { label: 'Active users',  value: '3,842',   delta: '+4.1%',  positive: true },
  { label: 'Churn rate',    value: '2.3%',    delta: '-0.4%',  positive: true },
  { label: 'Avg. session',  value: '4m 12s',  delta: '-8%',    positive: false },
]

<div style={{ display: 'grid', gridTemplateColumns: 'repeat(4, 1fr)', gap: 'var(--space-2)' }}>
  {stats.map(s => (
    <Card key={s.label}>
      <CardContent style={{ padding: 'var(--space-3)' }}>
        <p style={{ fontSize: 13, color: 'var(--text-secondary)', marginBottom: 4 }}>{s.label}</p>
        <p style={{ fontSize: 28, fontWeight: 700, lineHeight: 1 }}>{s.value}</p>
        <p style={{
          fontSize: 13, marginTop: 8,
          color: s.positive ? 'var(--success)' : 'var(--danger)',
        }}>
          {s.delta} vs last period
        </p>
      </CardContent>
    </Card>
  ))}
</div>
```

Sparkline variant: add a small recharts `<AreaChart>` (height 48) below the delta for trend context. Use `var(--accent)` for the line.

---

## Data table with filters

**Pattern: search + filter bar → table → pagination footer**

```jsx
<Card>
  {/* Filter bar */}
  <div style={{ padding: 'var(--space-2) var(--space-3)', borderBottom: '1px solid var(--border)', display: 'flex', gap: 'var(--space-1)' }}>
    <Input placeholder="Search…" style={{ width: 240 }} />
    <Select>
      <SelectTrigger style={{ width: 140 }}>
        <SelectValue placeholder="Status" />
      </SelectTrigger>
      <SelectContent>
        <SelectItem value="active">Active</SelectItem>
        <SelectItem value="inactive">Inactive</SelectItem>
      </SelectContent>
    </Select>
    <Button variant="outline" size="sm" style={{ marginLeft: 'auto' }}>Export</Button>
  </div>

  {/* Table */}
  <Table>
    <TableHeader>
      <TableRow>
        <TableHead>Name</TableHead>
        <TableHead>Status</TableHead>
        <TableHead>Created</TableHead>
        <TableHead style={{ textAlign: 'right' }}>Amount</TableHead>
      </TableRow>
    </TableHeader>
    <TableBody>
      {rows.map(row => (
        <TableRow key={row.id}>
          <TableCell style={{ fontWeight: 500 }}>{row.name}</TableCell>
          <TableCell>
            <Badge variant={row.status === 'active' ? 'default' : 'secondary'}>
              {row.status}
            </Badge>
          </TableCell>
          <TableCell style={{ color: 'var(--text-secondary)' }}>{row.date}</TableCell>
          <TableCell style={{ textAlign: 'right', fontVariantNumeric: 'tabular-nums' }}>
            {row.amount}
          </TableCell>
        </TableRow>
      ))}
    </TableBody>
  </Table>

  {/* Pagination */}
  <div style={{ padding: 'var(--space-2) var(--space-3)', borderTop: '1px solid var(--border)', display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
    <span style={{ fontSize: 13, color: 'var(--text-secondary)' }}>Showing 1–10 of 248</span>
    <div style={{ display: 'flex', gap: 4 }}>
      <Button variant="outline" size="sm">Previous</Button>
      <Button variant="outline" size="sm">Next</Button>
    </div>
  </div>
</Card>
```

---

## Empty state

**Never ship a plain spinner.** Every empty state needs an icon, a headline, a body, and an action.

```jsx
<div style={{
  display: 'flex', flexDirection: 'column', alignItems: 'center',
  justifyContent: 'center', padding: 'var(--space-12)',
  color: 'var(--text-secondary)', textAlign: 'center',
}}>
  <div style={{
    width: 56, height: 56, borderRadius: 'var(--radius-lg)',
    background: 'var(--bg-muted)', display: 'flex', alignItems: 'center',
    justifyContent: 'center', marginBottom: 'var(--space-3)',
    color: 'var(--text-tertiary)',
  }}>
    <FileText size={24} />
  </div>
  <h3 style={{ fontSize: 15, fontWeight: 600, color: 'var(--text-primary)', marginBottom: 6 }}>
    No reports yet
  </h3>
  <p style={{ fontSize: 14, maxWidth: 280, marginBottom: 'var(--space-3)' }}>
    Create your first report to start tracking performance across your team.
  </p>
  <Button size="sm">Create report</Button>
</div>
```

---

## Loading skeleton

Use `Skeleton` from shadcn. Match the skeleton shape to the real content layout.

```jsx
// Stat cards loading
<div style={{ display: 'grid', gridTemplateColumns: 'repeat(4, 1fr)', gap: 'var(--space-2)' }}>
  {[...Array(4)].map((_, i) => (
    <Card key={i}>
      <CardContent style={{ padding: 'var(--space-3)' }}>
        <Skeleton style={{ height: 13, width: '60%', marginBottom: 8 }} />
        <Skeleton style={{ height: 28, width: '80%', marginBottom: 8 }} />
        <Skeleton style={{ height: 13, width: '40%' }} />
      </CardContent>
    </Card>
  ))}
</div>

// Table rows loading
{[...Array(6)].map((_, i) => (
  <TableRow key={i}>
    <TableCell><Skeleton style={{ height: 14, width: '70%' }} /></TableCell>
    <TableCell><Skeleton style={{ height: 20, width: 60, borderRadius: 99 }} /></TableCell>
    <TableCell><Skeleton style={{ height: 14, width: '50%' }} /></TableCell>
    <TableCell><Skeleton style={{ height: 14, width: '40%', marginLeft: 'auto' }} /></TableCell>
  </TableRow>
))}
```

---

## Financial / trading dashboard panels

**Chart panel:**
```jsx
<Card>
  <CardHeader style={{ display: 'flex', flexDirection: 'row', alignItems: 'center', justifyContent: 'space-between' }}>
    <div>
      <h3 style={{ fontWeight: 600, fontSize: 15 }}>Portfolio value</h3>
      <p style={{ fontSize: 24, fontWeight: 700, marginTop: 2 }}>$284,921.50</p>
      <span style={{ fontSize: 13, color: 'var(--positive)' }}>↑ +$4,201 (1.5%) today</span>
    </div>
    <div style={{ display: 'flex', gap: 4 }}>
      {['1D','1W','1M','3M','1Y'].map(r => (
        <Button key={r} variant={r === '1M' ? 'default' : 'ghost'} size="sm" style={{ padding: '2px 8px', fontSize: 12 }}>{r}</Button>
      ))}
    </div>
  </CardHeader>
  <CardContent>
    {/* recharts AreaChart */}
    <AreaChart width={600} height={200} data={chartData}>
      <defs>
        <linearGradient id="fillValue" x1="0" y1="0" x2="0" y2="1">
          <stop offset="5%" stopColor="var(--accent)" stopOpacity={0.2} />
          <stop offset="95%" stopColor="var(--accent)" stopOpacity={0} />
        </linearGradient>
      </defs>
      <Area type="monotone" dataKey="value" stroke="var(--accent)" fill="url(#fillValue)" strokeWidth={2} dot={false} />
      <XAxis dataKey="date" tick={{ fontSize: 11, fill: 'var(--text-tertiary)' }} axisLine={false} tickLine={false} />
      <YAxis tick={{ fontSize: 11, fill: 'var(--text-tertiary)' }} axisLine={false} tickLine={false} />
      <Tooltip contentStyle={{ background: 'var(--bg-subtle)', border: '1px solid var(--border)', fontSize: 12 }} />
    </AreaChart>
  </CardContent>
</Card>
```

**Ticker row (positions list):**
```jsx
{positions.map(p => (
  <div key={p.symbol} style={{
    display: 'flex', alignItems: 'center', justifyContent: 'space-between',
    padding: 'var(--space-2) 0', borderBottom: '1px solid var(--border)',
  }}>
    <div style={{ display: 'flex', alignItems: 'center', gap: 'var(--space-2)' }}>
      <div style={{ width: 36, height: 36, borderRadius: 'var(--radius-md)', background: 'var(--bg-muted)', display: 'flex', alignItems: 'center', justifyContent: 'center', fontWeight: 700, fontSize: 12 }}>
        {p.symbol.slice(0,2)}
      </div>
      <div>
        <div style={{ fontWeight: 600, fontSize: 14 }}>{p.symbol}</div>
        <div style={{ color: 'var(--text-secondary)', fontSize: 12 }}>{p.name}</div>
      </div>
    </div>
    <div style={{ textAlign: 'right' }}>
      <div style={{ fontWeight: 600, fontSize: 14, fontVariantNumeric: 'tabular-nums' }}>{p.price}</div>
      <div style={{ fontSize: 12, color: p.change >= 0 ? 'var(--positive)' : 'var(--negative)' }}>
        {p.change >= 0 ? '↑' : '↓'} {Math.abs(p.change)}%
      </div>
    </div>
  </div>
))}
```

---

## Notification feed

```jsx
<div style={{ display: 'flex', flexDirection: 'column', gap: 1 }}>
  {notifications.map(n => (
    <div key={n.id} style={{
      display: 'flex', gap: 'var(--space-2)', padding: 'var(--space-2)',
      borderRadius: 'var(--radius-md)',
      background: n.unread ? 'var(--bg-subtle)' : 'transparent',
      cursor: 'pointer',
    }}>
      <Avatar style={{ width: 32, height: 32, flexShrink: 0 }}>
        <AvatarFallback style={{ fontSize: 12 }}>{n.initials}</AvatarFallback>
      </Avatar>
      <div style={{ flex: 1, minWidth: 0 }}>
        <p style={{ fontSize: 13, lineHeight: 1.4 }}>
          <strong>{n.actor}</strong> {n.action}
        </p>
        <p style={{ fontSize: 12, color: 'var(--text-tertiary)', marginTop: 2 }}>{n.time}</p>
      </div>
      {n.unread && (
        <div style={{ width: 8, height: 8, borderRadius: '50%', background: 'var(--accent)', flexShrink: 0, marginTop: 6 }} />
      )}
    </div>
  ))}
</div>
```
