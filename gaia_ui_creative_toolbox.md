# Gaia UI Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **Gaia UI** (ui.heygaia.io) — a library of premium agent-oriented card interfaces, loaders, custom lists, holo effects, and utility components built with Framer Motion, Tailwind CSS, and TypeScript.

## Table of Contents

- [Area Chart](#area-chart)
- [Author Tooltip](#author-tooltip)
- [Bar Chart](#bar-chart)
- [Calendar Event Card](#calendar-event-card)
- [Chat Demo](#chat-demo)
- [Code Block](#code-block)
- [Component Preview Tooltip](#component-preview-tooltip)
- [Chat Composer](#composer)
- [Email Compose Card](#email-compose-card)
- [File Dropzone](#file-dropzone)
- [File Preview](#file-preview)
- [Gauge Chart](#gauge-chart)
- [GitHub Stars Button](#github-stars-button)
- [Goal Card](#goal-card)
- [Holo Card](#holo-card)
- [iPhone Mockup](#iphone-mockup)
- [Knowledge Graph](#knowledge-graph)
- [Line Chart](#line-chart)
- [Link Preview](#link-preview)
- [iOS Message Bubbles](#message-bubble)
- [Model Selector](#model-selector)
- [Navbar Menu](#navbar-menu)
- [Nested Menu](#nested-menu)
- [Notification Card](#notification-card)
- [Pie Chart](#pie-chart)
- [Pricing Card](#pricing-card)
- [Radar Chart](#radar-chart)
- [Raised Button](#raised-button)
- [Scatter Chart](#scatter-chart)
- [Search Results Tabs](#search-results-tabs)
- [Slash Command Dropdown](#slash-command-dropdown)
- [Stat Row](#stat-row)
- [Todo Item](#todo-item)
- [Tool Calls Section](#tool-calls-section)
- [Twitter Card](#twitter-card)
- [Wave Spinner](#wave-spinner)
- [Weather Card](#weather-card)
- [Workflow Card](#workflow-card)

---

### Area Chart <a name="area-chart"></a>

A filled area chart with gradient fills for cumulative trends and stacked totals.

- **Registry Key**: `area-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/area-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `chart`
  - `card`

#### How to Use

```tsx
import { AreaChart } from "@/components/ui/area-chart";

const data = [
  { month: "Jan", users: 1200, revenue: 4200 },
  { month: "Feb", users: 1480, revenue: 5100 },
  { month: "Mar", users: 1620, revenue: 4800 },
];

export default function Example() {
  return (
    <AreaChart
      data={data}
      xKey="month"
      yKeys={["users", "revenue"]}
      title="Growth"
    />
  );
}
```

#### Props & API Reference

##### `AreaChart` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| data | Array&lt;Record&lt;string, string \| number&gt;&gt; | - | Chart data rows |
| xKey | string | - | Key on each row used for the X axis |
| yKeys | string \| string[] | - | One or more numeric keys to plot |
| title | string | - | Card title |
| description | string | - | Card description |
| footer | string | - | Optional footer text |
| colors | string[] | CHART_COLORS | Custom palette for series |

#### Component Source Code

##### File: `registry/new-york/ui/area-chart.tsx`

```tsx
"use client";

import type * as React from "react";
import {
	Area,
	AreaChart as RechartsAreaChart,
	XAxis,
	YAxis,
} from "recharts";
import { Card, CardContent, CardFooter, CardHeader } from "@/components/ui/card";
import {
	type ChartConfig,
	ChartContainer,
	ChartLegend,
	ChartLegendContent,
	ChartTooltip,
	ChartTooltipContent,
} from "@/components/ui/chart";

const CHART_COLORS = ["#00bbff", "#34d399", "#60a5fa", "#f472b6", "#fb923c"];

function toKeys(v: string | string[]): string[] {
	return Array.isArray(v) ? v : [v];
}

function ChartCard({
	title,
	description,
	footer,
	children,
	dataPoints = 0,
}: {
	title?: string;
	description?: string;
	footer?: string;
	children: React.ReactNode;
	dataPoints?: number;
}) {
	const width =
		dataPoints > 0 ? Math.min(768, Math.max(300, dataPoints * 80)) : undefined;
	return (
		<Card
			className="max-w-3xl"
			style={width ? { width } : { width: "100%" }}
		>
			{(title || description) && (
				<CardHeader className="pb-0">
					{title && (
						<p className="text-sm font-semibold text-card-foreground">
							{title}
						</p>
					)}
					{description && (
						<p className="text-xs text-muted-foreground">{description}</p>
					)}
				</CardHeader>
			)}
			<CardContent>{children}</CardContent>
			{footer && (
				<CardFooter>
					<p className="text-xs text-muted-foreground">{footer}</p>
				</CardFooter>
			)}
		</Card>
	);
}

export type AreaChartProps = {
	data: Array<Record<string, string | number>>;
	xKey: string;
	yKeys: string | string[];
	title?: string;
	description?: string;
	footer?: string;
	colors?: string[];
};

export function AreaChart(props: AreaChartProps) {
	const keys = toKeys(props.yKeys);
	const colors = props.colors ?? CHART_COLORS;
	const chartConfig: ChartConfig = Object.fromEntries(
		keys.map((key, i) => [
			key,
			{ label: key, color: colors[i % colors.length] },
		]),
	);
	const showLegend = keys.length > 1;
	return (
		<ChartCard
			title={props.title}
			description={props.description}
			footer={props.footer}
			dataPoints={props.data.length}
		>
			<ChartContainer config={chartConfig} className="h-50 w-full">
				<RechartsAreaChart data={props.data}>
					<defs>
						{keys.map((key) => (
							<linearGradient
								key={key}
								id={`gradient-${key}`}
								x1="0"
								y1="0"
								x2="0"
								y2="1"
							>
								<stop
									offset="0%"
									stopColor={`var(--color-${key})`}
									stopOpacity={0.4}
								/>
								<stop
									offset="95%"
									stopColor={`var(--color-${key})`}
									stopOpacity={0.05}
								/>
							</linearGradient>
						))}
					</defs>
					<XAxis
						dataKey={props.xKey}
						axisLine={false}
						tickLine={false}
						tick={{ fill: "#71717a", fontSize: 11 }}
					/>
					<YAxis
						axisLine={false}
						tickLine={false}
						tick={{ fill: "#71717a", fontSize: 11 }}
					/>
					<ChartTooltip content={<ChartTooltipContent />} />
					{showLegend && <ChartLegend content={<ChartLegendContent />} />}
					{keys.map((key) => (
						<Area
							key={key}
							type="natural"
							dataKey={key}
							stroke={`var(--color-${key})`}
							strokeWidth={2}
							fill={`url(#gradient-${key})`}
						/>
					))}
				</RechartsAreaChart>
			</ChartContainer>
		</ChartCard>
	);
}
```

---

### Author Tooltip <a name="author-tooltip"></a>

An interactive avatar with tooltip showing author information and social links.

- **Registry Key**: `author-tooltip`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/author-tooltip.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `icons`

#### How to Use

##### Example 1

```tsx
import { AuthorTooltip } from "@/components/ui/author-tooltip";

export default function Example() {
  const author = {
    name: "Aryan",
    avatar: "https://github.com/aryanranderiya.png",
    role: "Founder & CEO",
    github: "https://github.com/aryanranderiya",
    twitter: "https://twitter.com/aryanranderiya",
    linkedin: "https://linkedin.com/in/aryanranderiya",
  };

  return <AuthorTooltip author={author} />;
}
```

##### Example 2

```tsx
<AuthorTooltip 
  author={author}
  trigger={<button>Hover me</button>}
/>
```

##### Example 3

```tsx
<AuthorTooltip author={author}>
  <div className="text-xs">
    <p>Commits: 150</p>
    <p>Additions: +5,000</p>
  </div>
</AuthorTooltip>
```

#### Props & API Reference

##### `AuthorTooltip` Props

| Prop            | Type                       | Default                              | Description                               |
| --------------- | -------------------------- | ------------------------------------ | ----------------------------------------- |
| author          | Author                     | -                                    | Author information object                 |
| avatarSize      | "sm" \| "md" \| "lg" \| "xl" | "sm"                               | Size of the avatar                        |
| avatarClassName | string                     | "cursor-help border-2 border-border" | Additional CSS classes for avatar         |
| trigger         | ReactNode                  | -                                    | Custom trigger element (replaces avatar)  |
| children        | ReactNode                  | -                                    | Additional content to render in tooltip   |

#### Component Source Code

##### File: `registry/new-york/ui/author-tooltip.tsx`

```tsx
"use client";

import type * as React from "react";
import { FaGithub, FaLinkedin, FaTwitter } from "react-icons/fa";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import {
	Tooltip,
	TooltipContent,
	TooltipTrigger,
} from "@/components/ui/tooltip";
import { cn } from "@/lib/utils";

export interface Author {
	name: string;
	avatar: string;
	role: string;
	linkedin?: string;
	twitter?: string;
	github?: string;
}

interface AuthorTooltipProps {
	author: Author;
	avatarSize?: "sm" | "md" | "lg" | "xl";
	avatarClassName?: string;
	/** Custom trigger element - if not provided, defaults to avatar */
	trigger?: React.ReactNode;
	/** Additional content to render in the tooltip */
	children?: React.ReactNode;
}

const sizeMap = {
	sm: "h-8 w-8",
	md: "h-10 w-10",
	lg: "h-12 w-12",
	xl: "h-16 w-16",
};

export function AuthorTooltip({
	author,
	avatarSize = "sm",
	avatarClassName = "cursor-help border-2 border-border",
	trigger,
	children,
}: AuthorTooltipProps) {
	const getInitials = (name: string) => {
		return name
			.split(" ")
			.map((n) => n[0])
			.join("")
			.toUpperCase()
			.slice(0, 2);
	};

	const defaultTrigger = (
		<Avatar className={cn(sizeMap[avatarSize], avatarClassName)}>
			<AvatarImage src={author.avatar} alt={author.name} />
			<AvatarFallback>{getInitials(author.name)}</AvatarFallback>
		</Avatar>
	);

	return (
		<Tooltip>
			<TooltipTrigger asChild>
				{trigger ? <div>{trigger}</div> : defaultTrigger}
			</TooltipTrigger>
			<TooltipContent className="p-0 rounded-xl">
				<div className="flex flex-col gap-2 p-3">
					<div className="flex flex-row items-center gap-3">
						<Avatar className="h-10 w-10">
							<AvatarImage src={author.avatar} alt={author.name} />
							<AvatarFallback>{getInitials(author.name)}</AvatarFallback>
						</Avatar>
						<div className="flex flex-col">
							<span className="text-sm font-medium">{author.name}</span>
							<span className="text-xs text-muted-foreground">
								{author.role}
							</span>
						</div>
						{(author.linkedin || author.twitter || author.github) && (
							<div className="ml-4 flex gap-2.5">
								{author.linkedin && (
									<a
										href={author.linkedin}
										target="_blank"
										rel="noopener noreferrer"
										className="text-background hover:text-muted-foreground transition-colors"
									>
										<FaLinkedin className="h-5 w-5" />
									</a>
								)}
								{author.twitter && (
									<a
										href={author.twitter}
										target="_blank"
										rel="noopener noreferrer"
										className="text-background hover:text-muted-foreground transition-colors"
									>
										<FaTwitter className="h-5 w-5" />
									</a>
								)}
								{author.github && (
									<a
										href={author.github}
										target="_blank"
										rel="noopener noreferrer"
										className="text-background hover:text-muted-foreground transition-colors"
									>
										<FaGithub className="h-5 w-5" />
									</a>
								)}
							</div>
						)}
					</div>
					{children && <div>{children}</div>}
				</div>
			</TooltipContent>
		</Tooltip>
	);
}
```

---

### Bar Chart <a name="bar-chart"></a>

A bar chart for comparisons and distributions, with stacked, horizontal, and grouped variants.

- **Registry Key**: `bar-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/bar-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `chart`
  - `card`

#### How to Use

```tsx
import { BarChart } from "@/components/ui/bar-chart";

const data = [
  { month: "Jan", revenue: 4200 },
  { month: "Feb", revenue: 5100 },
  { month: "Mar", revenue: 4800 },
];

export default function Example() {
  return (
    <BarChart
      data={data}
      xKey="month"
      yKeys="revenue"
      title="Revenue"
      description="Monthly revenue"
    />
  );
}
```

#### Props & API Reference

##### `BarChart` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| data | Array&lt;Record&lt;string, string \| number&gt;&gt; | - | Chart data rows |
| xKey | string | - | Key on each row used for the category axis |
| yKeys | string \| string[] | - | One or more numeric keys to plot as bars |
| title | string | - | Card title |
| description | string | - | Card description |
| footer | string | - | Optional footer text |
| colors | string[] | CHART_COLORS | Custom palette for series |
| variant | "default" \| "stacked" \| "horizontal" \| "multiple" | "default" | Layout / grouping mode |

#### Component Source Code

##### File: `registry/new-york/ui/bar-chart.tsx`

```tsx
"use client";

import type * as React from "react";
import { Bar, BarChart as RechartsBarChart, XAxis, YAxis } from "recharts";
import { Card, CardContent, CardFooter, CardHeader } from "@/components/ui/card";
import {
	type ChartConfig,
	ChartContainer,
	ChartLegend,
	ChartLegendContent,
	ChartTooltip,
	ChartTooltipContent,
} from "@/components/ui/chart";

const CHART_COLORS = ["#00bbff", "#34d399", "#60a5fa", "#f472b6", "#fb923c"];

function toKeys(v: string | string[]): string[] {
	return Array.isArray(v) ? v : [v];
}

function ChartCard({
	title,
	description,
	footer,
	children,
	dataPoints = 0,
}: {
	title?: string;
	description?: string;
	footer?: string;
	children: React.ReactNode;
	dataPoints?: number;
}) {
	const width =
		dataPoints > 0 ? Math.min(768, Math.max(300, dataPoints * 80)) : undefined;
	return (
		<Card
			className="max-w-3xl"
			style={width ? { width } : { width: "100%" }}
		>
			{(title || description) && (
				<CardHeader className="pb-0">
					{title && (
						<p className="text-sm font-semibold text-card-foreground">
							{title}
						</p>
					)}
					{description && (
						<p className="text-xs text-muted-foreground">{description}</p>
					)}
				</CardHeader>
			)}
			<CardContent>{children}</CardContent>
			{footer && (
				<CardFooter>
					<p className="text-xs text-muted-foreground">{footer}</p>
				</CardFooter>
			)}
		</Card>
	);
}

export type BarChartProps = {
	data: Array<Record<string, string | number>>;
	xKey: string;
	yKeys: string | string[];
	title?: string;
	description?: string;
	footer?: string;
	colors?: string[];
	variant?: "default" | "stacked" | "horizontal" | "multiple";
};

export function BarChart(props: BarChartProps) {
	const keys = toKeys(props.yKeys);
	const colors = props.colors ?? CHART_COLORS;
	const chartConfig: ChartConfig = Object.fromEntries(
		keys.map((key, i) => [
			key,
			{ label: key, color: colors[i % colors.length] },
		]),
	);

	const isStacked = props.variant === "stacked";
	const isHorizontal = props.variant === "horizontal";
	const alwaysLegend = props.variant === "multiple";
	const showLegend = alwaysLegend || keys.length > 1;

	return (
		<ChartCard
			title={props.title}
			description={props.description}
			footer={props.footer}
			dataPoints={props.data.length}
		>
			<ChartContainer config={chartConfig} className="h-50 w-full">
				<RechartsBarChart
					data={props.data}
					layout={isHorizontal ? "vertical" : "horizontal"}
				>
					{isHorizontal ? (
						<>
							<YAxis
								dataKey={props.xKey}
								type="category"
								axisLine={false}
								tickLine={false}
								tick={{ fill: "#71717a", fontSize: 11 }}
								width={80}
							/>
							<XAxis
								type="number"
								axisLine={false}
								tickLine={false}
								tick={{ fill: "#71717a", fontSize: 11 }}
							/>
						</>
					) : (
						<>
							<XAxis
								dataKey={props.xKey}
								axisLine={false}
								tickLine={false}
								tick={{ fill: "#71717a", fontSize: 11 }}
							/>
							<YAxis
								axisLine={false}
								tickLine={false}
								tick={{ fill: "#71717a", fontSize: 11 }}
							/>
						</>
					)}
					<ChartTooltip cursor={false} content={<ChartTooltipContent />} />
					{showLegend && <ChartLegend content={<ChartLegendContent />} />}
					{keys.map((key) => (
						<Bar
							key={key}
							dataKey={key}
							fill={`var(--color-${key})`}
							radius={8}
							maxBarSize={40}
							{...(isStacked ? { stackId: "stack" } : {})}
						/>
					))}
				</RechartsBarChart>
			</ChartContainer>
		</ChartCard>
	);
}
```

---

### Calendar Event Card <a name="calendar-event-card"></a>

A styled card component for displaying calendar events with color indicators, status states, and action buttons.

- **Registry Key**: `calendar-event-card`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/calendar-event-card.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
import {
  CalendarEventCard,
  EventTitle,
  EventTime,
  EventLocation,
} from "@/components/ui/calendar-event-card";

export default function Example() {
  return (
    <CalendarEventCard eventColor="#3b82f6">
      <EventTitle>Team Meeting</EventTitle>
      <EventTime startTime="9:00 AM" endTime="10:00 AM" />
      <EventLocation>Conference Room A</EventLocation>
    </CalendarEventCard>
  );
}
```

#### Props & API Reference

##### `CalendarEventCard` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| eventColor | string | - | Color for the left indicator bar |
| status | "idle" \| "loading" \| "completed" | "idle" | Action button state |
| label | string | - | Optional label above content |
| variant | "display" \| "action" | "display" | Show action button when "action" |
| buttonColor | "primary" \| "danger" | "primary" | Action button color |
| completedLabel | string | "Completed" | Button text when completed |
| onAction | () => void | - | Action button click handler |
| isDotted | boolean | false | Show dotted border style |
| opacity | number | 1 | Card opacity (0-1) |

#### Component Source Code

##### File: `registry/new-york/ui/calendar-event-card.tsx`

```tsx

```

---

### Chat Demo <a name="chat-demo"></a>

A reusable platform-aware chat preview with pixel-accurate bubbles, headers and composers for iMessage, WhatsApp, Slack, Discord and Telegram.

- **Registry Key**: `chat-demo`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/chat-demo.json
```

#### How to Use

```tsx
import { ChatDemo } from "@/components/ui/chat-demo";
import { IPhoneMockup } from "@/components/ui/iphone-mockup";

export default function Example() {
  return (
    <IPhoneMockup>
      <ChatDemo
        platform="imessage"
        title="GAIA"
        messages={[
          { from: "them", text: "Morning ☀️", time: "9:41 AM" },
          { from: "me",   text: "Morning!", status: "read" },
        ]}
      />
    </IPhoneMockup>
  );
}
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| `platform` | `"imessage" \| "whatsapp" \| "slack" \| "discord" \| "telegram"` | – | Which platform's chrome and bubbles to render. |
| `messages` | `ChatMessageItem[]` | – | Messages to display. See shape below. |
| `title` | `string` | – | Contact name (or channel name for Slack / Discord). |
| `subtitle` | `string` | – | Header subtitle (e.g. `online`, `42 members`). |
| `headerAvatar` | `string` | github avatar | URL for the conversation avatar (iMessage / WhatsApp / Telegram). |
| `showComposer` | `boolean` | `true` | Toggle the bottom composer row. |
| `theme` | `"light" \| "dark"` | platform default | Force light/dark theme. Discord defaults to dark. |
| `className` | `string` | – | Extra classes for the outer wrapper. |

---

### Code Block <a name="code-block"></a>

A syntax-highlighted code block with copy and download functionality, perfect for displaying code snippets in documentation and tutorials.

- **Registry Key**: `code-block`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/code-block.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

##### Example 1

```tsx
import { CodeBlock } from "@/components/ui/code-block";

export default function Example() {
  const code = `function hello() {
  console.log("Hello, World!");
}`;

  return (
    <CodeBlock language="javascript" filename="hello.js">
      {code}
    </CodeBlock>
  );
}
```

##### Example 2

```tsx
import { Prism as SyntaxHighlighter } from 'react-syntax-highlighter';
import { oneDark } from 'react-syntax-highlighter/dist/esm/styles/prism';

<CodeBlock language="javascript">
  <SyntaxHighlighter language="javascript" style={oneDark}>
    {code}
  </SyntaxHighlighter>
</CodeBlock>
```

##### Example 3

```tsx
<CodeBlock 
  className="border-blue-500 bg-blue-950" 
  language="python"
>
  {code}
</CodeBlock>
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| children | string | - | The code content to display |
| language | string | "plaintext" | Programming language for syntax highlighting |
| filename | string | - | Optional filename to display in header |
| showLineNumbers | boolean | false | Show line numbers on the left |
| className | string | - | Additional CSS classes |

#### Component Source Code

##### File: `registry/new-york/ui/code-block.tsx`

```tsx

```

---

### Component Preview Tooltip <a name="component-preview-tooltip"></a>

A hover tooltip that shows a live preview of any component, perfect for navigation menus and component galleries.

- **Registry Key**: `component-preview-tooltip`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/component-preview-tooltip.json
```

#### How to Use

```tsx
import { ComponentPreviewTooltip } from "@/components/ui/component-preview-tooltip";

// In your sidebar or component list
<ComponentPreviewTooltip componentName="todo-item" side="right">
  <Link href="/docs/components/todo-item">
    Todo Item
  </Link>
</ComponentPreviewTooltip>
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| componentName | string | - | Name of the component (matches preview folder name) |
| children | ReactNode | - | Element that triggers the tooltip on hover |
| side | "left" \| "right" \| "top" \| "bottom" | "right" | Tooltip position |

#### Component Source Code

##### File: `registry/new-york/ui/component-preview-tooltip.tsx`

```tsx

```

---

### Chat Composer <a name="composer"></a>

A chat message input component with file attachments, auto-growing textarea, and tool integration support.

- **Registry Key**: `composer`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/composer.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `icons`
  - `file-preview`
  - `slash-command-dropdown`

#### How to Use

##### Example 1

```tsx
import { Composer } from "@/components/ui/composer";

export default function Chat() {
  return (
    <Composer
      placeholder="What can I do for you today?"
      onSubmit={(message, files) => {
        console.log("Message:", message);
        console.log("Files:", files);
      }}
    />
  );
}
```

##### Example 2

```tsx
import { Composer, Tool } from "@/components/ui/composer";

const tools: Tool[] = [
  {
    name: "web_search",
    category: "search",
    description: "Search the web for information",
  },
  {
    name: "image_generation",
    category: "creative",
    description: "Generate images from text descriptions",
  },
];

export default function ChatWithTools() {
  return (
    <Composer
      tools={tools}
      onToolSelect={(tool) => {
        console.log("Selected tool:", tool);
      }}
      onSubmit={(message) => {
        console.log("Sent:", message);
      }}
    />
  );
}
```

##### Example 3

```tsx
import { Composer, ComposerContextOption } from "@/components/ui/composer";
import { HugeiconsIcon, AttachmentIcon, Image01Icon } from "@/components/icons";

const contextOptions: ComposerContextOption[] = [
  {
    id: "attach",
    label: "Attach Files",
    description: "Upload documents or images",
    icon: <HugeiconsIcon icon={AttachmentIcon} size={18} />,
    onClick: () => openFilePicker(),
  },
  {
    id: "image",
    label: "Add Image",
    description: "Upload or generate an image",
    icon: <HugeiconsIcon icon={Image01Icon} size={18} />,
    onClick: () => openImagePicker(),
  },
];

export default function ChatWithContext() {
  return (
    <Composer
      contextOptions={contextOptions}
      onSubmit={(message) => console.log(message)}
    />
  );
}
```

##### Example 4

```tsx
import { useState } from "react";
import { Composer, UploadedFile } from "@/components/ui/composer";

export default function ChatWithFiles() {
  const [files, setFiles] = useState<UploadedFile[]>([]);

  return (
    <Composer
      attachedFiles={files}
      onRemoveFile={(id) => {
        setFiles((prev) => prev.filter((f) => f.id !== id));
      }}
      onSubmit={(message, files) => {
        console.log("Sending:", message, files);
        setFiles([]);
      }}
    />
  );
}
```

#### Props & API Reference

##### `Composer` Props

| Prop             | Type                                              | Default                           | Description                                  |
| ---------------- | ------------------------------------------------- | --------------------------------- | -------------------------------------------- |
| placeholder      | string                                            | "What can I do for you today?"    | Placeholder text for the input              |
| onSubmit         | (message: string, files?: UploadedFile[]) => void | -                                 | Callback when message is submitted          |
| onChange         | (value: string) => void                           | -                                 | Callback when input value changes           |
| disabled         | boolean                                           | false                             | Whether the composer is disabled            |
| showToolsButton  | boolean                                           | true                              | Whether to show the tools button            |
| tools            | Tool[]                                            | []                                | Available tools for slash command dropdown  |
| onToolSelect     | (tool: Tool) => void                              | -                                 | Callback when a tool is selected            |
| contextOptions   | ComposerContextOption[]                           | -                                 | Options for the plus button dropdown        |
| onAttachClick    | () => void                                        | -                                 | Callback when attach clicked (no options)   |
| autoFocus        | boolean                                           | false                             | Whether to auto-focus the input             |
| maxRows          | number                                            | 8                                 | Maximum rows for the textarea               |
| defaultValue     | string                                            | ""                                | Initial uncontrolled value                  |
| value            | string                                            | -                                 | Controlled value                            |
| className        | string                                            | -                                 | Additional className for the container      |
| attachedFiles    | UploadedFile[]                                    | []                                | Attached files to display                   |
| onRemoveFile     | (id: string) => void                              | -                                 | Callback to remove an attached file         |

#### Component Source Code

##### File: `registry/new-york/ui/composer.tsx`

```tsx
"use client";

import type { FC, ReactNode } from "react";
import { useCallback, useEffect, useRef, useState } from "react";
import {
	ArrowUp02Icon,
	HugeiconsIcon,
	PlusSignIcon,
	ToolsIcon,
} from "@/components/icons";
import { cn } from "@/lib/utils";
import {
	FilePreview,
	type UploadedFile,
} from "@/registry/new-york/ui/file-preview";
import {
	SlashCommandDropdown,
	type SlashCommandMatch,
	type Tool,
} from "@/registry/new-york/ui/slash-command-dropdown";

// Re-export types for convenience
export type { Tool, SlashCommandMatch, UploadedFile };

export interface ComposerContextOption {
	id: string;
	label: string;
	icon?: ReactNode;
	description?: string;
	onClick?: () => void;
}

export interface ComposerProps {
	/** Placeholder text for the input */
	placeholder?: string;
	/** Callback when message is submitted */
	onSubmit?: (message: string, files?: UploadedFile[]) => void;
	/** Callback when input value changes */
	onChange?: (value: string) => void;
	/** Whether the composer is disabled */
	disabled?: boolean;
	/** Whether to show the tools button */
	showToolsButton?: boolean;
	/** Callback when a tool is selected from slash command dropdown */
	onToolSelect?: (tool: Tool) => void;
	/** Available tools for the slash command dropdown */
	tools?: Tool[];
	/** Callback when attach button is clicked (if no context options) */
	onAttachClick?: () => void;
	/** Context options for the plus button dropdown */
	contextOptions?: ComposerContextOption[];
	/** Whether to auto-focus the input */
	autoFocus?: boolean;
	/** Maximum rows for the textarea */
	maxRows?: number;
	/** Initial value */
	defaultValue?: string;
	/** Controlled value */
	value?: string;
	/** Additional className for the container */
	className?: string;
	/** Attached files to display */
	attachedFiles?: UploadedFile[];
	/** Callback to remove an attached file */
	onRemoveFile?: (id: string) => void;
	/** Whether the composer is in loading state */
	isLoading?: boolean;
}

// Primary color matching GAIA: #00bbff
const PRIMARY_COLOR = "#00bbff";

export const Composer: FC<ComposerProps> = ({
	placeholder = "What can I do for you today?",
	onSubmit,
	onChange,
	disabled = false,
	showToolsButton = true,
	onToolSelect,
	tools = [],
	onAttachClick,
	contextOptions,
	autoFocus = false,
	maxRows = 8,
	defaultValue = "",
	value,
	className,
	attachedFiles = [],
	onRemoveFile,
	isLoading = false,
}) => {
	const [inputValue, setInputValue] = useState(defaultValue);
	const [isToolsDropdownOpen, setIsToolsDropdownOpen] = useState(false);
	const [isContextMenuOpen, setIsContextMenuOpen] = useState(false);
	const [selectedCategory, setSelectedCategory] = useState("all");
	const selectedToolIndex = 0; // Currently no keyboard navigation, always start at 0
	const textareaRef = useRef<HTMLTextAreaElement>(null);
	const composerRef = useRef<HTMLDivElement>(null);

	// Use controlled or uncontrolled value
	const currentValue = value !== undefined ? value : inputValue;

	// Handle input change
	const handleInputChange = useCallback(
		(e: React.ChangeEvent<HTMLTextAreaElement>) => {
			const newValue = e.target.value;
			if (value === undefined) {
				setInputValue(newValue);
			}
			onChange?.(newValue);
		},
		[onChange, value],
	);

	// Auto-resize textarea
	// biome-ignore lint/correctness/useExhaustiveDependencies: currentValue triggers resize when content changes
	useEffect(() => {
		const textarea = textareaRef.current;
		if (!textarea) return;

		textarea.style.height = "auto";
		const lineHeight = 24;
		const maxHeight = lineHeight * maxRows;
		const newHeight = Math.min(textarea.scrollHeight, maxHeight);
		textarea.style.height = `${newHeight}px`;
	}, [currentValue, maxRows]);

	// Handle form submit
	const handleSubmit = useCallback(
		(e?: React.FormEvent) => {
			e?.preventDefault();
			if (isLoading) return;
			if (currentValue.trim() || attachedFiles.length > 0) {
				onSubmit?.(currentValue, attachedFiles);
				if (value === undefined) {
					setInputValue("");
				}
			}
		},
		[currentValue, attachedFiles, onSubmit, value, isLoading],
	);

	// Handle key down
	const handleKeyDown = useCallback(
		(e: React.KeyboardEvent<HTMLTextAreaElement>) => {
			if (e.key === "Enter" && !e.shiftKey && !disabled && !isLoading) {
				e.preventDefault();
				handleSubmit();
			}
			if (e.key === "Escape") {
				setIsToolsDropdownOpen(false);
				setIsContextMenuOpen(false);
			}
		},
		[handleSubmit, disabled, isLoading],
	);

	// Handle tools button click
	const handleToolsClick = useCallback(() => {
		if (isLoading) return;
		setIsToolsDropdownOpen(!isToolsDropdownOpen);
		setIsContextMenuOpen(false);
	}, [isToolsDropdownOpen, isLoading]);

	// Handle context menu click
	const handleContextClick = useCallback(() => {
		if (isLoading) return;
		if (contextOptions && contextOptions.length > 0) {
			setIsContextMenuOpen(!isContextMenuOpen);
			setIsToolsDropdownOpen(false);
		} else {
			onAttachClick?.();
		}
	}, [contextOptions, isContextMenuOpen, onAttachClick, isLoading]);

	// Handle tool selection
	const handleToolSelect = useCallback(
		(match: SlashCommandMatch) => {
			onToolSelect?.(match.tool);
			setIsToolsDropdownOpen(false);
		},
		[onToolSelect],
	);

	// Close dropdowns when clicking outside
	useEffect(() => {
		const handleClickOutside = (event: MouseEvent) => {
			const target = event.target as Element;
			if (
				!composerRef.current?.contains(target) &&
				!target.closest(".slash-command-dropdown")
			) {
				setIsToolsDropdownOpen(false);
				setIsContextMenuOpen(false);
			}
		};

		document.addEventListener("click", handleClickOutside);
		return () => document.removeEventListener("click", handleClickOutside);
	}, []);

	// Focus input on mount if autoFocus is true
	useEffect(() => {
		if (autoFocus && textareaRef.current) {
			textareaRef.current.focus();
		}
	}, [autoFocus]);

	// Convert tools to matches for dropdown
	const toolMatches: SlashCommandMatch[] = tools.map((tool) => ({
		tool,
		score: 1,
	}));

	// Get unique categories
	const categories = ["all", ...new Set(tools.map((t) => t.category))];

	const canSubmit = currentValue.trim() || attachedFiles.length > 0;

	return (
		<div className={cn("relative w-full", className)}>
			{/* Main Composer Container - light mode friendly */}
			<div
				ref={composerRef}
				className={cn(
					"relative rounded-3xl px-1 pt-1 pb-2",
					// Light mode: light gray background, Dark mode: zinc-800
					"bg-zinc-100 dark:bg-zinc-800",
				)}
			>
				{/* Slash Command Dropdown - positioned above composer */}
				{showToolsButton && tools.length > 0 && isToolsDropdownOpen && (
					<div className="absolute bottom-full left-0 right-0 mb-2 z-50">
						<SlashCommandDropdown
							matches={toolMatches}
							selectedIndex={selectedToolIndex}
							onSelect={handleToolSelect}
							onClose={() => setIsToolsDropdownOpen(false)}
							position={{ left: 0, width: undefined }}
							isVisible={true}
							openedViaButton={true}
							selectedCategory={selectedCategory}
							categories={categories}
							onCategoryChange={setSelectedCategory}
							className="relative w-full"
							style={{ position: "relative" }}
						/>
					</div>
				)}
				{/* Attached Files Preview - using FilePreview component */}
				<FilePreview
					files={attachedFiles}
					onRemove={onRemoveFile}
					className="rounded-xl"
				/>

				{/* Textarea Input */}
				<form onSubmit={handleSubmit}>
					<div className="relative px-3">
						<textarea
							ref={textareaRef}
							value={currentValue}
							onChange={handleInputChange}
							onKeyDown={handleKeyDown}
							placeholder={placeholder}
							disabled={disabled || isLoading}
							rows={1}
							className={cn(
								"w-full resize-none bg-transparent py-3 pr-24 transition-all text-base font-light",
								// Light mode text colors
								"text-zinc-900 dark:text-white",
								"placeholder:text-zinc-400 dark:placeholder:text-zinc-500",
								"focus:outline-none",
								"disabled:cursor-not-allowed disabled:opacity-50",
								"scrollbar-thin scrollbar-thumb-zinc-300 dark:scrollbar-thumb-zinc-600 scrollbar-track-transparent",
							)}
							style={{
								minHeight: "24px",
								maxHeight: `${24 * maxRows}px`,
							}}
						/>
						{/* Hint for slash commands */}
						<div className="absolute right-3 top-1/2 -translate-y-1/2 flex items-center gap-1 text-xs text-zinc-400 dark:text-zinc-500 pointer-events-none">
							<kbd className="rounded bg-zinc-200 dark:bg-zinc-700 px-1.5 py-0.5 text-[10px] font-medium text-zinc-500 dark:text-zinc-400">
								/
							</kbd>
							<span>for tools</span>
						</div>
					</div>
				</form>

				{/* Toolbar */}
				<div className="flex items-center justify-between px-2 pt-1">
					<div className="flex items-center gap-1">
						{/* Add Context / Attach Button */}
						<div className="relative">
							<button
								type="button"
								onClick={handleContextClick}
								disabled={disabled || isLoading}
								className={cn(
									"group relative flex h-9 w-9 items-center justify-center rounded-full cursor-pointer",
									// Light mode colors
									"bg-zinc-200 dark:bg-zinc-700 transition-colors",
									"hover:bg-zinc-300 dark:hover:bg-zinc-600/90",
									"disabled:cursor-wait disabled:opacity-70",
									isContextMenuOpen && "bg-zinc-300 dark:bg-zinc-600",
								)}
								aria-label="Add context or attach files"
							>
								<HugeiconsIcon
									icon={PlusSignIcon}
									size={23}
									className="text-zinc-500 dark:text-zinc-400"
								/>
							</button>

							{/* Context Menu Dropdown */}
							{isContextMenuOpen && contextOptions && (
								<div className="absolute bottom-full left-0 mb-2 min-w-[200px] rounded-xl border border-zinc-200 dark:border-zinc-700 bg-white dark:bg-zinc-900 p-1 shadow-xl overflow-hidden animate-in fade-in-0 slide-in-from-bottom-2 duration-150">
									{contextOptions.map((option) => (
										<button
											key={option.id}
											type="button"
											onClick={() => {
												option.onClick?.();
												setIsContextMenuOpen(false);
											}}
											className="flex w-full cursor-pointer items-center gap-2 rounded-lg px-3 py-2 text-left text-sm text-zinc-900 dark:text-white hover:bg-zinc-100 dark:hover:bg-zinc-800 transition-colors"
										>
											{option.icon && (
												<span
													className="flex-shrink-0"
													style={{ color: PRIMARY_COLOR }}
												>
													{option.icon}
												</span>
											)}
											<div className="flex flex-col">
												<span>{option.label}</span>
												{option.description && (
													<span className="text-xs text-zinc-500 dark:text-zinc-400">
														{option.description}
													</span>
												)}
											</div>
										</button>
									))}
								</div>
							)}
						</div>

						{/* Tools Button */}
						{showToolsButton && (
							<button
								type="button"
								onClick={handleToolsClick}
								disabled={disabled || isLoading}
								className={cn(
									"group relative flex h-9 w-9 items-center justify-center rounded-full cursor-pointer",
									// Light mode colors
									"bg-zinc-200 dark:bg-zinc-700 text-zinc-500 dark:text-zinc-400 transition-colors",
									"hover:bg-zinc-300 dark:hover:bg-zinc-600/90",
									"disabled:cursor-wait disabled:opacity-70",
									// Active state
									isToolsDropdownOpen &&
										"bg-[#00bbff]/20 text-[#00bbff] hover:bg-[#00bbff]/30 dark:hover:bg-[#00bbff]/40",
								)}
								aria-label="Browse all tools"
							>
								<HugeiconsIcon icon={ToolsIcon} size={23} />
								{isToolsDropdownOpen && (
									<span
										className="absolute top-0 right-0 h-2 w-2 rounded-full transition"
										style={{ backgroundColor: PRIMARY_COLOR }}
										aria-hidden="true"
									/>
								)}
							</button>
						)}
					</div>

					{/* Send Button */}
					<button
						type="button"
						onClick={() => handleSubmit()}
						disabled={disabled || isLoading || !canSubmit}
						className={cn(
							"flex h-9 w-9 min-w-9 max-w-9 items-center justify-center rounded-full transition-colors cursor-pointer",
							"disabled:cursor-not-allowed",
							// Enabled state - primary color background
							canSubmit && "bg-[#00bbff] text-white hover:bg-[#00a3e0]",
							// Disabled state - visible gray background that contrasts with composer
							!canSubmit &&
								"bg-zinc-200 dark:bg-zinc-700 text-zinc-400 dark:text-zinc-500",
						)}
						aria-label="Send message"
					>
						<HugeiconsIcon icon={ArrowUp02Icon} size={20} />
					</button>
				</div>
			</div>
		</div>
	);
};

export default Composer;
```

---

### Email Compose Card <a name="email-compose-card"></a>

An AI-powered email composition card with recipient validation, smart inputs, and beautiful styling.

- **Registry Key**: `email-compose-card`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/email-compose-card.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `icons`

#### Props & API Reference

##### `EmailComposeCardProps` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| subject | string | - | The subject line content |
| body | string | - | The body content |
| recipients | string[] | [] | Initial list of recipient emails |
| mode | "view" \| "edit" | "edit" | Current display mode |
| recipientQuery | string | - | Optional context string (e.g., prompt used to generate email) |
| onSubjectChange | (value: string) => void | - | Callback when subject changes |
| onBodyChange | (value: string) => void | - | Callback when body changes |
| onRecipientsChange | (recipients: string[]) => void | - | Callback when recipients change |
| onSend | () => void | - | Callback for send action |
| onCancel | () => void | - | Callback for cancel action |
| isSending | boolean | false | Loading state for send action |

#### Component Source Code

##### File: `registry/new-york/ui/email-compose-card.tsx`

```tsx
"use client";

import {
	Button,
	Chip,
	Input,
	Modal,
	ModalBody,
	ModalContent,
	ScrollShadow,
	Textarea,
} from "@heroui/react";
import type React from "react";
import { useEffect, useRef, useState } from "react";
import { z } from "zod";
import {
	Cancel01Icon,
	HugeiconsIcon,
	Loading03Icon,
	Mail01Icon,
	PencilEdit01Icon,
	PlusSignIcon,
	Tick02Icon,
} from "@/components/icons";
import { cn } from "@/lib/utils";

// Email validation schema
const emailComposeSchema = z.object({
	to: z
		.array(z.string().email("Invalid email address"))
		.min(1, "At least one recipient is required"),
	subject: z
		.string()
		.min(1, "Subject is required")
		.max(200, "Subject must be under 200 characters"),
	body: z
		.string()
		.min(1, "Email body is required")
		.max(10000, "Email body must be under 10,000 characters"),
});

const emailValidationSchema = z.string().email("Invalid email address");

export interface EmailData {
	to: string[];
	subject: string;
	body: string;
	draft_id?: string;
	thread_id?: string;
	bcc?: string[];
	cc?: string[];
	is_html?: boolean;
}

export interface EmailComposeCardProps {
	emailData: EmailData;
	onSend?: (data: EmailData) => void;
	className?: string;
}

function RecipientSelectionModal({
	isOpen,
	onClose,
	onConfirm,
	suggestions,
	selectedEmails,
	setSelectedEmails,
	customEmailInput,
	setCustomEmailInput,
	customEmailError,
	setCustomEmailError,
	handleAddCustomEmail,
}: {
	isOpen: boolean;
	onClose: () => void;
	onConfirm: () => void;
	suggestions: string[];
	selectedEmails: string[];
	setSelectedEmails: React.Dispatch<React.SetStateAction<string[]>>;
	customEmailInput: string;
	setCustomEmailInput: React.Dispatch<React.SetStateAction<string>>;
	customEmailError: string;
	setCustomEmailError: React.Dispatch<React.SetStateAction<string>>;
	handleAddCustomEmail: () => void;
}) {
	const handleSuggestionToggle = (email: string) => {
		setSelectedEmails((prev) => {
			if (prev.includes(email)) {
				// Remove from selected
				return prev.filter((e) => e !== email);
			} else {
				// Add to selected
				return [...prev, email];
			}
		});
	};

	const handleCustomEmailKeyPress = (e: React.KeyboardEvent) => {
		if (e.key === "Enter") {
			e.preventDefault();
			handleAddCustomEmail();
		}
	};

	return (
		<Modal isOpen={isOpen} onOpenChange={onClose} size="sm">
			<ModalContent>
				<ModalBody>
					<div className="pt-2 text-sm font-medium text-zinc-900 dark:text-zinc-100">
						Email Suggestions
					</div>

					{/* Suggestions */}
					<div className="flex flex-wrap gap-2">
						{suggestions.map((email) => (
							<Chip
								key={email}
								size="sm"
								variant="flat"
								color={selectedEmails.includes(email) ? "primary" : "default"}
								className="cursor-pointer text-xs"
								onClick={() => handleSuggestionToggle(email)}
								endContent={
									selectedEmails.includes(email) ? (
										<HugeiconsIcon icon={Cancel01Icon} size={12} />
									) : null
								}
							>
								{email}
							</Chip>
						))}
					</div>

					<hr className="my-2 border-zinc-200 dark:border-zinc-700" />

					<div className="flex gap-2">
						<Input
							placeholder="Add email..."
							value={customEmailInput}
							onChange={(e) => {
								setCustomEmailInput(e.target.value);
								setCustomEmailError("");
							}}
							onKeyDown={handleCustomEmailKeyPress}
							size="sm"
							isInvalid={!!customEmailError}
							errorMessage={customEmailError}
						/>
						<Button
							size="sm"
							color="primary"
							onPress={handleAddCustomEmail}
							isIconOnly
						>
							<HugeiconsIcon icon={PlusSignIcon} size={16} />
						</Button>
					</div>

					{/* Action Buttons */}
					<div className="mt-4 flex justify-end gap-2">
						<Button variant="light" size="sm" onPress={onClose}>
							Cancel
						</Button>
						<Button
							color="primary"
							size="sm"
							onPress={onConfirm}
							isDisabled={selectedEmails.length === 0}
						>
							Done ({selectedEmails.length})
						</Button>
					</div>
				</ModalBody>
			</ModalContent>
		</Modal>
	);
}

export function EmailComposeCard({
	emailData,
	onSend,
	className,
}: EmailComposeCardProps) {
	const [isRecipientModalOpen, setIsRecipientModalOpen] = useState(false);
	const [isSending, setIsSending] = useState(false);

	// Inline editing states
	const [isEditingSubject, setIsEditingSubject] = useState(false);
	const [isEditingBody, setIsEditingBody] = useState(false);

	const [editData, setEditData] = useState<EmailData>(emailData);

	// Suggestions come from emailData.to - these are resolved email addresses from the agent
	const [suggestions, setSuggestions] = useState<string[]>(emailData.to || []);

	// Selected emails state
	const [selectedEmails, setSelectedEmails] = useState<string[]>(
		emailData.to || [],
	);

	// Custom email input state
	const [customEmailInput, setCustomEmailInput] = useState("");
	const [customEmailError, setCustomEmailError] = useState("");

	// Input refs for auto-focus
	const subjectInputRef = useRef<HTMLInputElement>(null);
	const bodyInputRef = useRef<HTMLTextAreaElement>(null);

	// Initialize with empty emails array - user must select recipients
	// If there's only one email, select it by default
	useEffect(() => {
		const suggestions = emailData.to || [];
		setEditData((prev) => ({ ...prev, to: [] }));
		setSuggestions(suggestions);

		// If there's exactly one email suggestion, select it by default
		if (suggestions.length === 1) setSelectedEmails([suggestions[0]]);
		else setSelectedEmails([]);
	}, [emailData.to]);

	// Auto-focus logic
	useEffect(() => {
		if (isEditingSubject && subjectInputRef.current) {
			subjectInputRef.current.focus();
		}
	}, [isEditingSubject]);

	useEffect(() => {
		if (isEditingBody && bodyInputRef.current) {
			bodyInputRef.current.focus();
		}
	}, [isEditingBody]);

	const validateForm = () => {
		try {
			emailComposeSchema.parse({
				to: selectedEmails,
				subject: editData.subject,
				body: editData.body,
			});

			return true;
		} catch (error) {
			if (error instanceof z.ZodError) {
				const newErrors: Record<string, string> = {};
				error.errors.forEach((err) => {
					if (err.path[0]) {
						newErrors[err.path[0].toString()] = err.message;
					}
				});
			}
			return false;
		}
	};

	const validateCustomEmail = (email: string): boolean => {
		try {
			emailValidationSchema.parse(email);
			setCustomEmailError("");
			return true;
		} catch (error) {
			if (error instanceof z.ZodError) {
				setCustomEmailError(error.errors[0]?.message || "Invalid email");
			}
			return false;
		}
	};

	const handleSend = async () => {
		if (selectedEmails.length === 0) {
			console.error("Please select at least one recipient");
			return;
		}

		if (!validateForm()) {
			console.error("Please fix the validation errors");
			return;
		}

		setIsSending(true);
		try {
			if (onSend) {
				await onSend({ ...editData, to: selectedEmails });
			}
		} catch (error) {
			console.error("Error sending email:", error);
		} finally {
			setIsSending(false);
		}
	};

	// Handle custom email addition
	const handleAddCustomEmail = () => {
		const trimmedEmail = customEmailInput.trim();

		if (!trimmedEmail) {
			setCustomEmailError("Please enter an email address");
			return;
		}

		if (!validateCustomEmail(trimmedEmail)) {
			return;
		}

		if (selectedEmails.includes(trimmedEmail)) {
			setCustomEmailError("Email already selected");
			return;
		}

		// Add to selected emails
		setSelectedEmails((prev) => [...prev, trimmedEmail]);

		// Add to suggestions if not already there
		if (!suggestions.includes(trimmedEmail)) {
			setSuggestions((prev) => [...prev, trimmedEmail]);
		}

		// Clear input
		setCustomEmailInput("");
		setCustomEmailError("");
	};

	const handleConfirmRecipients = () => {
		setEditData((prev) => ({ ...prev, to: selectedEmails }));
		setIsRecipientModalOpen(false);
	};

	return (
		<>
			{/* Main Email Card - Zinc Colors, Flat & Rounded, Light/Dark Mode */}
			<div
				className={cn(
					"w-full max-w-xl overflow-hidden rounded-3xl",
					"bg-zinc-100 dark:bg-zinc-900",
					className,
				)}
			>
				{/* Header with status chip */}
				<div className="flex items-center justify-between px-6 py-1">
					<div className="flex flex-row items-center gap-2 pt-3 pb-2 text-zinc-900 dark:text-zinc-100">
						<HugeiconsIcon icon={Mail01Icon} size={18} />
						<span className="text-sm font-medium">
							{emailData.draft_id ? "Email Draft" : "Compose Email"}
						</span>
						{emailData.thread_id && (
							<Chip size="sm" variant="flat" color="primary">
								Reply
							</Chip>
						)}
					</div>
				</div>
				<div className="flex flex-col gap-1 px-6">
					<div className="flex items-center gap-2 text-sm text-zinc-500 dark:text-zinc-400">
						<span>To:</span>
						<span className="flex w-full items-center justify-between font-medium text-zinc-900 dark:text-zinc-200">
							{selectedEmails.join(", ") || ""}
							<Button
								size="sm"
								onPress={() => setIsRecipientModalOpen(true)}
								variant={selectedEmails.length === 0 ? "flat" : "light"}
								isIconOnly={selectedEmails.length !== 0}
								className={cn(
									selectedEmails.length === 0
										? ""
										: "text-zinc-500 dark:text-zinc-400",
								)}
								endContent={
									selectedEmails.length === 0 ? (
										""
									) : (
										<HugeiconsIcon icon={PencilEdit01Icon} size={20} />
									)
								}
							>
								{selectedEmails.length === 0 ? "Add Recipients" : ``}
							</Button>
						</span>
					</div>
					<div className="my-1.5 h-px bg-zinc-200 dark:bg-zinc-800" />
					<div className="flex w-full items-center justify-between text-sm text-zinc-500 dark:text-zinc-400">
						<div className="flex items-center gap-2 w-full">
							<span className="flex-shrink-0">Subject:</span>
							{isEditingSubject ? (
								<Input
									ref={subjectInputRef}
									value={editData.subject}
									onChange={(e) =>
										setEditData({ ...editData, subject: e.target.value })
									}
									onBlur={() => setIsEditingSubject(false)}
									onKeyDown={(e) => {
										if (e.key === "Enter") setIsEditingSubject(false);
									}}
									size="sm"
									classNames={{
										input:
											"text-base font-medium text-zinc-900 dark:text-zinc-200",
										inputWrapper:
											"h-7 min-h-7 bg-transparent shadow-none border-none px-0",
									}}
								/>
							) : (
								<span className="font-medium text-zinc-900 dark:text-zinc-200 truncate flex-1">
									{editData.subject}
								</span>
							)}
						</div>

						<Button
							variant="light"
							size="sm"
							isIconOnly
							onPress={() => setIsEditingSubject(!isEditingSubject)}
							className="text-zinc-500 dark:text-zinc-400"
						>
							{isEditingSubject ? (
								<HugeiconsIcon icon={Tick02Icon} size={20} />
							) : (
								<HugeiconsIcon icon={PencilEdit01Icon} size={20} />
							)}
						</Button>
					</div>
					<div className="my-1.5 h-px bg-zinc-200 dark:bg-zinc-800" />

					<ScrollShadow className="relative z-[1] max-h-46 min-h-[150px] overflow-y-auto pb-5 text-sm leading-relaxed whitespace-pre-line text-zinc-800 dark:text-zinc-200">
						<div className="absolute top-0 right-0 z-[2] flex w-full justify-end">
							<Button
								variant="light"
								size="sm"
								isIconOnly
								onPress={() => setIsEditingBody(!isEditingBody)}
								className="text-zinc-500 dark:text-zinc-400"
							>
								{isEditingBody ? (
									<HugeiconsIcon icon={Tick02Icon} size={20} />
								) : (
									<HugeiconsIcon icon={PencilEdit01Icon} size={20} />
								)}
							</Button>
						</div>

						{isEditingBody ? (
							<Textarea
								ref={bodyInputRef}
								value={editData.body}
								onChange={(e) =>
									setEditData({ ...editData, body: e.target.value })
								}
								onBlur={() => setIsEditingBody(false)}
								minRows={6}
								classNames={{
									input:
										"text-sm text-zinc-900 dark:text-zinc-200 placeholder:text-zinc-400 dark:placeholder:text-zinc-500",
									inputWrapper: "bg-transparent shadow-none border-none p-0",
								}}
							/>
						) : (
							editData.body
						)}
					</ScrollShadow>
				</div>
				<div className="flex justify-end px-6 pb-5">
					<Button
						color="primary"
						onPress={handleSend}
						isLoading={isSending}
						isDisabled={selectedEmails.length === 0}
						radius="full"
						className="font-medium"
					>
						{isSending ? (
							<>
								<HugeiconsIcon
									icon={Loading03Icon}
									size={16}
									className="animate-spin"
								/>
								Sending...
							</>
						) : emailData.draft_id ? (
							"Send Draft"
						) : (
							"Send"
						)}
					</Button>
				</div>
			</div>

			<RecipientSelectionModal
				isOpen={isRecipientModalOpen}
				onClose={() => setIsRecipientModalOpen(false)}
				onConfirm={handleConfirmRecipients}
				suggestions={suggestions}
				selectedEmails={selectedEmails}
				setSelectedEmails={setSelectedEmails}
				customEmailInput={customEmailInput}
				setCustomEmailInput={setCustomEmailInput}
				customEmailError={customEmailError}
				setCustomEmailError={setCustomEmailError}
				handleAddCustomEmail={handleAddCustomEmail}
			/>
		</>
	);
}
```

---

### File Dropzone <a name="file-dropzone"></a>

A drag-and-drop file upload zone with preview, validation, and file management.

- **Registry Key**: `file-dropzone`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/file-dropzone.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
import { FileDropzone } from "@/components/ui/file-dropzone";

export default function Example() {
  return (
    <FileDropzone
      onFilesDropped={(files) => console.log(files)}
      accept="image/*,.pdf"
      maxSize={5 * 1024 * 1024}
      maxFiles={5}
    />
  );
}
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| onFilesDropped | (files) => void | - | Callback with dropped files |
| accept | string | - | Accepted file types |
| multiple | boolean | true | Allow multiple files |
| maxSize | number | 10MB | Max file size in bytes |
| maxFiles | number | 10 | Maximum number of files |
| disabled | boolean | false | Disable the dropzone |

#### Component Source Code

##### File: `registry/new-york/ui/file-dropzone.tsx`

```tsx

```

---

### File Preview <a name="file-preview"></a>

Display uploaded files with smart type detection, file icons, image thumbnails, and upload progress indicators.

- **Registry Key**: `file-preview`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/file-preview.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
import { FilePreview } from "@/components/ui/file-preview";

export default function Example() {
  const files = [
    {
      id: "1",
      url: "/uploads/document.pdf",
      name: "report.pdf",
      type: "application/pdf",
    },
  ];

  return (
    <FilePreview
      files={files}
      onRemove={(id) => console.log("Remove:", id)}
    />
  );
}
```

#### Component Source Code

##### File: `registry/new-york/ui/file-preview.tsx`

```tsx

```

---

### Gauge Chart <a name="gauge-chart"></a>

A radial gauge for a single value with thresholds, plus text and stacked variants.

- **Registry Key**: `gauge-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/gauge-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `chart`

#### How to Use

```tsx
import { GaugeChart } from "@/components/ui/gauge-chart";

export default function Example() {
  return (
    <GaugeChart
      value={68}
      title="Memory"
      unit="%"
      thresholds={{ warning: 60, danger: 80 }}
    />
  );
}
```

#### Props & API Reference

##### `GaugeChart` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| value | number | - | Primary value to render |
| title | string | - | Label below the value |
| min | number | 0 | Lower bound of the range |
| max | number | 100 | Upper bound of the range |
| unit | string | - | Optional unit suffix (e.g. "%") |
| thresholds | `{ warning: number; danger: number }` | `{ warning: 60, danger: 80 }` | Percentage thresholds for color switching |
| variant | "gauge" \| "text" \| "stacked" | "gauge" | Visual style |
| secondValue | number | - | Secondary value (stacked variant only) |
| secondLabel | string | - | Secondary label (stacked variant only) |

#### Component Source Code

##### File: `registry/new-york/ui/gauge-chart.tsx`

```tsx
"use client";

import {
	Label,
	PolarGrid,
	PolarRadiusAxis,
	RadialBar,
	RadialBarChart,
} from "recharts";
import {
	type ChartConfig,
	ChartContainer,
} from "@/components/ui/chart";

const CHART_COLORS = ["#00bbff", "#34d399", "#60a5fa", "#f472b6", "#fb923c"];

export type GaugeChartProps = {
	value: number;
	title?: string;
	min?: number;
	max?: number;
	unit?: string;
	thresholds?: { warning: number; danger: number };
	variant?: "gauge" | "text" | "stacked";
	secondValue?: number;
	secondLabel?: string;
};

export function GaugeChart(props: GaugeChartProps) {
	const min = props.min ?? 0;
	const max = props.max ?? 100;
	const pct = Math.min(
		100,
		Math.max(0, ((props.value - min) / (max - min)) * 100),
	);
	const warning = props.thresholds?.warning ?? 60;
	const danger = props.thresholds?.danger ?? 80;
	const color =
		pct >= danger ? "#f87171" : pct >= warning ? "#fbbf24" : "#34d399";

	const variant = props.variant ?? "gauge";

	if (variant === "text") {
		const textChartConfig: ChartConfig = {
			value: { label: props.title ?? "Value", color },
		};
		const textData = [{ name: "value", value: pct, fill: color }];
		return (
			<div className="rounded-2xl bg-card text-card-foreground border p-4 text-center">
				<ChartContainer config={textChartConfig} className="mx-auto">
					<RadialBarChart
						data={textData}
						startAngle={0}
						endAngle={250}
						outerRadius={90}
						innerRadius={70}
						className="mx-auto h-[160px] w-[160px]"
					>
						<PolarGrid
							gridType="circle"
							radialLines={false}
							stroke="none"
							className="first:fill-muted last:fill-background"
							polarRadius={[90, 70]}
						/>
						<RadialBar dataKey="value" background cornerRadius={10} />
						<PolarRadiusAxis tick={false} tickLine={false} axisLine={false}>
							<Label
								content={({ viewBox }) => {
									if (viewBox && "cx" in viewBox && "cy" in viewBox) {
										return (
											<text
												x={viewBox.cx}
												y={viewBox.cy}
												textAnchor="middle"
												dominantBaseline="middle"
											>
												<tspan
													x={viewBox.cx}
													y={viewBox.cy}
													fontSize={28}
													fontWeight="bold"
													fill={color}
												>
													{props.value}
													{props.unit ?? ""}
												</tspan>
												{props.title && (
													<tspan
														x={viewBox.cx}
														y={(viewBox.cy ?? 0) + 22}
														fontSize={12}
														fill="#71717a"
													>
														{props.title}
													</tspan>
												)}
											</text>
										);
									}
								}}
							/>
						</PolarRadiusAxis>
					</RadialBarChart>
				</ChartContainer>
			</div>
		);
	}

	if (variant === "stacked") {
		const total = props.value + (props.secondValue ?? 0);
		const stackData = [
			{ primary: props.value, secondary: props.secondValue ?? 0 },
		];
		const stackChartConfig: ChartConfig = {
			primary: { label: props.title ?? "Primary", color },
			secondary: {
				label: props.secondLabel ?? "Secondary",
				color: CHART_COLORS[1],
			},
		};
		return (
			<div className="rounded-2xl bg-card text-card-foreground border p-4 text-center">
				<ChartContainer config={stackChartConfig} className="mx-auto">
					<RadialBarChart
						data={stackData}
						endAngle={180}
						innerRadius={80}
						outerRadius={110}
						className="mx-auto h-[140px] w-[200px]"
					>
						<RadialBar
							dataKey="secondary"
							fill={CHART_COLORS[1]}
							stackId="a"
							cornerRadius={5}
							className="stroke-transparent stroke-2"
						/>
						<RadialBar
							dataKey="primary"
							fill={color}
							stackId="a"
							cornerRadius={5}
							className="stroke-transparent stroke-2"
						/>
						<PolarRadiusAxis tick={false} tickLine={false} axisLine={false}>
							<Label
								content={({ viewBox }) => {
									if (viewBox && "cx" in viewBox && "cy" in viewBox) {
										return (
											<text x={viewBox.cx} y={viewBox.cy} textAnchor="middle">
												<tspan
													x={viewBox.cx}
													y={(viewBox.cy ?? 0) - 12}
													fontSize={22}
													fontWeight="bold"
													className="fill-foreground"
												>
													{total.toLocaleString()}
												</tspan>
												<tspan
													x={viewBox.cx}
													y={(viewBox.cy ?? 0) + 8}
													fontSize={12}
													fill="#71717a"
												>
													{props.title ?? "Total"}
												</tspan>
											</text>
										);
									}
								}}
							/>
						</PolarRadiusAxis>
					</RadialBarChart>
				</ChartContainer>
			</div>
		);
	}

	// Default: "gauge" variant
	const maxArc = 250;
	const valueAngle = Math.max(1, (pct / 100) * maxArc);
	const outerR = 65;
	const innerR = 55;

	const gaugeConfig: ChartConfig = {
		value: { label: props.title ?? "Value", color },
	};
	const gaugeData = [{ name: "value", value: 1, fill: color }];

	return (
		<div
			className="rounded-2xl bg-card text-card-foreground border p-4 text-center"
			style={{ width: 200 }}
		>
			<ChartContainer
				config={gaugeConfig}
				className="mx-auto h-[160px] w-[160px]"
			>
				<RadialBarChart
					data={gaugeData}
					startAngle={0}
					endAngle={valueAngle}
					outerRadius={outerR}
					innerRadius={innerR}
				>
					<PolarGrid
						gridType="circle"
						radialLines={false}
						stroke="none"
						className="first:fill-muted last:fill-background"
						polarRadius={[outerR, innerR]}
					/>
					<RadialBar dataKey="value" cornerRadius={10} />
					<PolarRadiusAxis tick={false} tickLine={false} axisLine={false}>
						<Label
							content={({ viewBox }) => {
								if (viewBox && "cx" in viewBox && "cy" in viewBox) {
									return (
										<text
											x={viewBox.cx}
											y={viewBox.cy}
											textAnchor="middle"
											dominantBaseline="middle"
										>
											<tspan
												x={viewBox.cx}
												y={viewBox.cy}
												fontSize={28}
												fontWeight="bold"
												fill={color}
											>
												{props.value}
												{props.unit ?? ""}
											</tspan>
											{props.title && (
												<tspan
													x={viewBox.cx}
													y={(viewBox.cy ?? 0) + 22}
													fontSize={12}
													fill="#71717a"
												>
													{props.title}
												</tspan>
											)}
										</text>
									);
								}
							}}
						/>
					</PolarRadiusAxis>
				</RadialBarChart>
			</ChartContainer>
		</div>
	);
}
```

---

### GitHub Stars Button <a name="github-stars-button"></a>

A beautiful button that displays your GitHub repository's star count with real-time data fetching.

- **Registry Key**: `github-stars-button`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/github-stars-button.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-icons`
- **Local Registry Dependencies**:
  - `raised-button`

#### How to Use

##### Example 1

```tsx
import { GitHubStarsButton } from "@/components/ui/github-stars-button";

export default function Example() {
  return <GitHubStarsButton repo="theexperiencecompany/gaia" />;
}
```

##### Example 2

```tsx
<GitHubStarsButton repo="vercel/next.js" />
<GitHubStarsButton repo="facebook/react" />
<GitHubStarsButton repo="microsoft/vscode" />
```

##### Example 3

```tsx
<GitHubStarsButton repo="theexperiencecompany/gaia" showLabel={false} />
```

##### Example 4

```tsx
<GitHubStarsButton repo="theexperiencecompany/gaia" size="sm" />
<GitHubStarsButton repo="theexperiencecompany/gaia" size="default" />
<GitHubStarsButton repo="theexperiencecompany/gaia" size="lg" />
```

##### Example 5

```tsx
<GitHubStarsButton
  repo="theexperiencecompany/gaia"
  className="hover:scale-105 transition-transform"
/>
```

#### Props & API Reference

##### `Props` Props

| Prop      | Type                      | Default   | Description                    |
| --------- | ------------------------- | --------- | ------------------------------ |
| repo      | string                    | -         | GitHub repository (owner/repo) |
| showLabel | boolean                   | true      | Whether to show "GitHub" label |
| size      | "sm" \| "default" \| "lg" | "sm"      | Size of the button             |
| className | string                    | undefined | Additional CSS classes         |

#### Component Source Code

##### File: `registry/new-york/ui/github-stars-button.tsx`

```tsx
"use client";

import { StarFilledIcon } from "@radix-ui/react-icons";
import { useEffect, useState } from "react";
import { Github } from "@/components/icons/social-icons";
import { RaisedButton } from "@/registry/new-york/ui/raised-button";

interface GitHubRepo {
	stargazers_count: number;
	html_url: string;
	name: string;
	full_name: string;
}

interface GitHubStarsButtonProps {
	repo: string;
	showLabel?: boolean;
	size?: "sm" | "default" | "lg";
	className?: string;
}

export function GitHubStarsButton({
	repo,
	showLabel = true,
	size = "sm",
	className,
}: GitHubStarsButtonProps) {
	const [starCount, setStarCount] = useState<number | null>(null);
	const [isLoading, setIsLoading] = useState(true);

	useEffect(() => {
		let isMounted = true;

		async function fetchStars() {
			try {
				const response = await fetch(`https://api.github.com/repos/${repo}`);

				if (!response.ok) {
					throw new Error("Failed to fetch repository data");
				}

				const data: GitHubRepo = await response.json();

				if (isMounted) {
					setStarCount(data.stargazers_count);
					setIsLoading(false);
				}
			} catch (error) {
				console.error("Error fetching GitHub stars:", error);
				if (isMounted) {
					setIsLoading(false);
				}
			}
		}

		fetchStars();

		return () => {
			isMounted = false;
		};
	}, [repo]);

	return (
		<a
			href={`https://github.com/${repo}`}
			target="_blank"
			rel="noopener noreferrer"
		>
			<RaisedButton
				size={size}
				className={`group rounded-xl border-0! ${className || ""}`}
				color="#1c1c1c"
			>
				<div className="flex items-center">
					<Github />
					{showLabel && <span className="ml-1">GitHub</span>}
				</div>
				<div className="flex items-center gap-1 text-sm">
					<StarFilledIcon className="relative top-px size-4 text-white group-hover:text-yellow-300" />
					<span className="font-medium text-white">
						{isLoading ? "..." : starCount || "0"}
					</span>
				</div>
			</RaisedButton>
		</a>
	);
}
```

---

### Goal Card <a name="goal-card"></a>

A card component for displaying goal progress with status indicators and step tracking.

- **Registry Key**: `goal-card`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/goal-card.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
import { GoalCard } from "@/components/ui/goal-card";

export default function Example() {
  return (
    <GoalCard
      id="1"
      title="Launch MVP"
      progress={75}
      status="in_progress"
      steps={[
        { id: "1", title: "Design", completed: true },
        { id: "2", title: "Develop", completed: false },
      ]}
      onClick={(id) => console.log(id)}
    />
  );
}
```

#### Component Source Code

##### File: `registry/new-york/ui/goal-card.tsx`

```tsx

```

---

### Holo Card <a name="holo-card"></a>

An interactive 3D holographic profile card with tilt effects, sparkle animations, and flip functionality.

- **Registry Key**: `holo-card`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/holo-card.json
```

#### Dependencies

- **NPM Packages**:
  - `react-parallax-tilt`

#### How to Use

##### Example 1

```tsx
import { HoloCard } from "@/components/ui/holo-card";

const profileData = {
  name: "John Doe",
  subtitle: "The Innovator",
  description: "Building the future one line of code at a time.",
  primaryId: "00042",
  secondaryInfo: "December 2024",
  badge: "Premium",
  backgroundImage: "https://example.com/background.jpg",
  overlayColor: "#4f46e5",
  overlayOpacity: 30,
};

export default function Example() {
  return <HoloCard data={profileData} />;
}
```

##### Example 2

```tsx
<HoloCard data={profileData} forceSide="front" />
```

##### Example 3

```tsx
<HoloCard
  data={profileData}
  branding={{
    logo: "/my-logo.svg",
    logoAlt: "My Brand",
    icon: "/my-icon.svg",
    iconAlt: "Brand Icon",
  }}
/>
```

##### Example 4

```tsx
<HoloCard data={profileData} showSparkles={false} />
```

##### Example 5

```tsx
<HoloCard data={profileData} width={240} height={336} />
```

#### Props & API Reference

##### `HoloCard` Props

| Prop         | Type                     | Default   | Description                                         |
| ------------ | ------------------------ | --------- | --------------------------------------------------- |
| data         | HoloCardDisplayData      | -         | Card display data (required)                        |
| width        | number                   | 320       | Card width in pixels                                |
| height       | number                   | 446       | Card height in pixels                               |
| showSparkles | boolean                  | true      | Show sparkle animation effect                       |
| forceSide    | "front" \| "back"        | undefined | Force card to show specific side (disables flip)   |
| branding     | HoloCardBranding         | undefined | Custom branding (logos, icons)                      |
| className    | string                   | undefined | Additional CSS classes                              |
| onFlip       | (isFlipped: boolean) => void | undefined | Callback when card is flipped                  |

#### Component Source Code

##### File: `registry/new-york/ui/holo-card.tsx`

```tsx
"use client";

import Image from "next/image";
import type React from "react";
import { useRef, useState, useCallback } from "react";
import Tilt from "react-parallax-tilt";
import { cn } from "@/lib/utils";
import "./holo-card.css";

// ============================================================================
// Types
// ============================================================================

export interface HoloCardDisplayData {
	/** Primary name displayed on the card */
	name: string;
	/** Secondary text (e.g., personality phrase, tagline) */
	subtitle?: string;
	/** Extended description shown on the back of the card */
	description?: string;
	/** Primary identifier (e.g., user number, ID) */
	primaryId?: string | number;
	/** Secondary info (e.g., member since date) */
	secondaryInfo?: string;
	/** Background image URL */
	backgroundImage?: string;
	/** Badge/tag text (e.g., house name, role) */
	badge?: string;
	/** Overlay color for the card */
	overlayColor?: string;
	/** Overlay opacity (0-100) */
	overlayOpacity?: number;
}

export interface HoloCardBranding {
	/** Primary logo image URL */
	logo?: string;
	/** Logo alt text */
	logoAlt?: string;
	/** Icon shown in various places */
	icon?: string;
	/** Icon alt text */
	iconAlt?: string;
}

export interface HoloCardProps {
	/** Card display data */
	data: HoloCardDisplayData;
	/** Card dimensions */
	height?: number;
	width?: number;
	/** Show sparkle animation effect */
	showSparkles?: boolean;
	/** Force card to show front or back (disables flip) */
	forceSide?: "front" | "back";
	/** Custom branding (logos, icons) */
	branding?: HoloCardBranding;
	/** Additional CSS classes */
	className?: string;
	/** Children rendered inside the card */
	children?: React.ReactNode;
	/** Callback when card is flipped */
	onFlip?: (isFlipped: boolean) => void;
}

// ============================================================================
// Default Values
// ============================================================================

const DEFAULT_BACKGROUND =
	"https://i.pinimg.com/1200x/27/0a/74/270a74bdc412f9eeae4d2403ebc9bd63.jpg";

const DEFAULT_BRANDING: HoloCardBranding = {
	logo: "/images/logos/text_w_logo_white.webp",
	logoAlt: "Logo",
	icon: "/images/logos/experience_logo.svg",
	iconAlt: "Icon",
};

// ============================================================================
// Component
// ============================================================================

export const HoloCard = ({
	data,
	height = 446,
	width = 320,
	showSparkles = true,
	forceSide,
	branding = DEFAULT_BRANDING,
	className,
	children,
	onFlip,
}: HoloCardProps) => {
	const [hover, setHover] = useState(false);
	const [animated, setAnimated] = useState(true);
	const [isFlipped, setIsFlipped] = useState(false);
	const [activeBackgroundPosition, setActiveBackgroundPosition] = useState({
		tp: 0,
		lp: 0,
	});
	const ref = useRef<HTMLDivElement>(null);

	const {
		name,
		subtitle,
		description,
		primaryId,
		secondaryInfo,
		backgroundImage = DEFAULT_BACKGROUND,
		badge,
		overlayColor,
		overlayOpacity = 40,
	} = data;

	const handleCardClick = useCallback(() => {
		if (!forceSide) {
			const newFlippedState = !isFlipped;
			setIsFlipped(newFlippedState);
			onFlip?.(newFlippedState);
		}
	}, [forceSide, isFlipped, onFlip]);

	const handleOnMouseMove = useCallback(
		(event: React.MouseEvent<HTMLDivElement>) => {
			setAnimated(false);
			setHover(true);

			const card = ref.current;
			const l = event.nativeEvent.offsetX;
			const t = event.nativeEvent.offsetY;

			const h = card ? card.clientHeight : 0;
			const w = card ? card.clientWidth : 0;

			const px = Math.abs(Math.floor((100 / w) * l) - 100);
			const py = Math.abs(Math.floor((100 / h) * t) - 100);

			const lp = 50 + (px - 50) / 1.5;
			const tp = 50 + (py - 50) / 1.5;

			setActiveBackgroundPosition({ lp, tp });
		},
		[],
	);

	const handleOnTouchMove = useCallback(
		(event: React.TouchEvent<HTMLDivElement>) => {
			setAnimated(false);
			setHover(true);

			const card = ref.current;
			if (!card) return;

			const touch = event.touches[0];
			const rect = card.getBoundingClientRect();
			const l = touch.clientX - rect.left;
			const t = touch.clientY - rect.top;

			const h = card.clientHeight;
			const w = card.clientWidth;

			const px = Math.abs(Math.floor((100 / w) * l) - 100);
			const py = Math.abs(Math.floor((100 / h) * t) - 100);

			const lp = 50 + (px - 50) / 1.5;
			const tp = 50 + (py - 50) / 1.5;

			setActiveBackgroundPosition({ lp, tp });
		},
		[],
	);

	const handleOnMouseOut = useCallback(() => {
		setHover(false);
		setAnimated(true);
	}, []);

	const effectiveFlipped = forceSide ? forceSide === "back" : isFlipped;

	// Static mode styles for download/static rendering
	const containerStyle = forceSide
		? {
				perspective: "none",
				transform: "none",
			}
		: {
				perspective: "1000px",
				cursor: "pointer",
			};

	const innerStyle = forceSide
		? {
				transform: "none",
				position: "relative" as const,
				height: `${height}px`,
				width: `${width}px`,
			}
		: {
				transformStyle: "preserve-3d" as const,
				transform: effectiveFlipped ? "rotateY(180deg)" : "rotateY(0deg)",
				height: `${height}px`,
				width: `${width}px`,
			};

	const frontStyle = forceSide
		? {
				display: forceSide === "front" ? "block" : "none",
				position: "absolute" as const,
				inset: 0,
			}
		: {
				position: "absolute" as const,
				inset: 0,
				backfaceVisibility: "hidden" as const,
				WebkitBackfaceVisibility: "hidden" as const,
			};

	const backStyle = forceSide
		? {
				display: forceSide === "back" ? "block" : "none",
				position: "absolute" as const,
				inset: 0,
				transform: "none",
			}
		: {
				position: "absolute" as const,
				inset: 0,
				backfaceVisibility: "hidden" as const,
				WebkitBackfaceVisibility: "hidden" as const,
				transform: "rotateY(180deg)",
			};

	const holoCardClasses = cn(
		"holo-card",
		hover && "holo-card--active",
		animated && "holo-card--animated",
		showSparkles && "holo-card--sparkles",
	);

	const holoCardStyle = {
		"--holo-width": `${width}px`,
		"--holo-height": `${height}px`,
		backgroundImage: `url(${backgroundImage})`,
		backgroundPosition: hover
			? `${activeBackgroundPosition.lp}% ${activeBackgroundPosition.tp}%`
			: "center",
	} as React.CSSProperties;

	// Overlay component
	const OverlayLayer = overlayColor ? (
		<div
			className="pointer-events-none absolute inset-0 z-[3]"
			style={{
				background: overlayColor,
				mixBlendMode: "overlay",
				opacity: overlayOpacity / 100,
			}}
		/>
	) : null;

	// Front card content
	const FrontContent = (
		<div className="pointer-events-none absolute z-[2] flex h-full w-full flex-col items-start justify-end p-3 text-white transition">
			{/* Top bar with logo and badge */}
			<div className="absolute top-4 left-0 flex w-full justify-between px-3">
				{branding.logo && (
					<div className="rounded-full bg-white/30 p-1 px-2 font-serif text-xl font-light text-white/70 backdrop-blur-md">
						<Image
							src={branding.logo}
							alt={branding.logoAlt || "Logo"}
							width={100}
							height={30}
							className="object-contain"
						/>
					</div>
				)}
				{badge && (
					<div className="rounded-full bg-white/20 p-1 px-4 font-serif text-xl font-light text-white/70 backdrop-blur-md">
						{badge}
					</div>
				)}
			</div>

			{/* Background icon */}
			{branding.icon && (
				<Image
					src={branding.icon}
					alt={branding.iconAlt || "Icon"}
					className="scale-125 opacity-10"
					fill
				/>
			)}

			{/* Bottom info card */}
			<div className="relative flex w-full flex-col gap-1 overflow-hidden rounded-2xl bg-black/20 p-3 backdrop-blur-md">
				<div className="font-serif text-4xl font-bold text-white">{name}</div>
				{subtitle && (
					<div className="mb-10 font-light text-white italic">{subtitle}</div>
				)}

				<div className="flex w-full items-center justify-between">
					<div className="flex flex-col items-start gap-1">
						{primaryId && (
							<span className="text-sm text-white/80">User {primaryId}</span>
						)}
						{secondaryInfo && (
							<span className="text-xs text-white/50">{secondaryInfo}</span>
						)}
					</div>

					{branding.icon && (
						<div className="flex gap-2">
							<Image
								src={branding.icon}
								alt={branding.iconAlt || "Icon"}
								width={30}
								height={30}
							/>
						</div>
					)}
				</div>
			</div>
		</div>
	);

	// Back card content
	const BackContent = (
		<div className="pointer-events-none absolute z-[2] flex h-full w-full flex-col items-start justify-between p-3 text-white">
			<div className="flex w-full flex-col gap-4">
				{/* Top bar */}
				<div className="flex items-center justify-between">
					{branding.logo && (
						<div className="rounded-full bg-white/30 p-1 px-2 backdrop-blur-md">
							<Image
								src={branding.logo}
								alt={branding.logoAlt || "Logo"}
								width={80}
								height={24}
								className="object-contain"
							/>
						</div>
					)}
					{badge && (
						<div className="rounded-full bg-white/20 p-1 px-3 font-serif text-xl font-light text-white/70 backdrop-blur-md">
							{badge}
						</div>
					)}
				</div>

				{/* Bio section */}
				<div className="relative overflow-hidden rounded-2xl bg-black/20 p-4 backdrop-blur-md">
					<div className="mb-2 font-serif text-2xl font-bold text-white">
						{name}
					</div>
					{subtitle && (
						<div className="mb-4 text-sm font-light text-white/80 italic">
							{subtitle}
						</div>
					)}
					{description && (
						<p className="text-sm text-white/80">{description}</p>
					)}
				</div>
			</div>

			{/* Bottom info */}
			<div className="flex w-full items-center justify-between rounded-xl bg-black/20 p-3 backdrop-blur-md">
				<div className="flex flex-col gap-1">
					<span className="text-xs text-white/50">Member Since</span>
					<span className="text-sm font-medium text-white/80">
						{secondaryInfo || "—"}
					</span>
				</div>
				<div className="flex flex-col items-end gap-1">
					<span className="text-xs text-white/50">User ID</span>
					<span className="text-sm font-medium text-white/80">
						{primaryId || "—"}
					</span>
				</div>
			</div>
		</div>
	);

	// Render card face (either static or with tilt)
	const renderCardFace = (
		content: React.ReactNode,
		isStatic: boolean = false,
	) => {
		if (isStatic || forceSide) {
			return (
				<div className="relative h-full w-full overflow-hidden rounded-2xl shadow-xl">
					{OverlayLayer}
					{content}
					<div ref={ref} className={holoCardClasses} style={holoCardStyle}>
						{children}
					</div>
				</div>
			);
		}

		return (
			<Tilt className="relative h-full w-full overflow-hidden rounded-2xl p-0! shadow-xl">
				{OverlayLayer}
				{content}
				<div
					ref={ref}
					className={holoCardClasses}
					style={holoCardStyle}
					onMouseMove={handleOnMouseMove}
					onTouchMove={handleOnTouchMove}
					onMouseOut={handleOnMouseOut}
					onBlur={handleOnMouseOut}
				>
					{children}
				</div>
			</Tilt>
		);
	};

	const handleKeyDown = useCallback(
		(event: React.KeyboardEvent) => {
			if (event.key === "Enter" || event.key === " ") {
				event.preventDefault();
				handleCardClick();
			}
		},
		[handleCardClick],
	);

	return (
		<div
			className={cn(forceSide ? "" : "perspective-1000", className)}
			onClick={handleCardClick}
			onKeyDown={!forceSide ? handleKeyDown : undefined}
			role={!forceSide ? "button" : undefined}
			tabIndex={!forceSide ? 0 : undefined}
			title={
				!forceSide ? `Profile card for ${name}. Click to flip.` : undefined
			}
			style={containerStyle}
		>
			<div
				className={
					forceSide ? "relative" : "relative transition-transform duration-700"
				}
				style={innerStyle}
			>
				{/* Front Side */}
				<div style={frontStyle}>
					{renderCardFace(FrontContent, !!forceSide)}
				</div>

				{/* Back Side */}
				<div style={backStyle}>{renderCardFace(BackContent, !!forceSide)}</div>
			</div>
		</div>
	);
};

export default HoloCard;
```

##### File: `registry/new-york/ui/holo-card.css`

```tsx
/* HoloCard Animations and Styles */

@keyframes holoSparkle {
	0%,
	100% {
		opacity: 0.75;
		filter: brightness(1.2) contrast(1.25);
	}
	5%,
	8% {
		opacity: 1;
		filter: brightness(0.8) contrast(1.2);
	}
	13%,
	16% {
		opacity: 0.5;
		filter: brightness(1.2) contrast(0.8);
	}
	35%,
	38% {
		opacity: 1;
		filter: brightness(1) contrast(1);
	}
	55% {
		opacity: 0.33;
		filter: brightness(1.2) contrast(1.25);
	}
}

@keyframes holoGradient {
	0%,
	100% {
		opacity: 0.5;
		background-position: 50% 50%;
		filter: brightness(0.5) contrast(1);
	}
	5%,
	9% {
		background-position: 100% 100%;
		opacity: 1;
		filter: brightness(0.75) contrast(1.25);
	}
	13%,
	17% {
		background-position: 0% 0%;
		opacity: 0.88;
	}
	35%,
	39% {
		background-position: 100% 100%;
		opacity: 1;
		filter: brightness(0.5) contrast(1);
	}
	55% {
		background-position: 0% 0%;
		opacity: 1;
		filter: brightness(0.75) contrast(1.25);
	}
}

.holo-card {
	--holo-width: 320px;
	--holo-height: 446px;

	width: var(--holo-width);
	height: var(--holo-height);
	background-color: #211799;
	background-size: cover;
	background-repeat: no-repeat;
	background-position: center;
	border-radius: 5% / 3.5%;
	box-shadow:
		-3px -3px 3px 0 rgba(38, 230, 247, 0.3),
		3px 3px 3px 0 rgba(247, 89, 228, 0.3),
		0 0 6px 2px rgba(255, 231, 89, 0.3),
		0 35px 25px -15px rgba(0, 0, 0, 0.3);
	position: relative;
	overflow: hidden;
	display: inline-block;
	vertical-align: middle;
	transition: transform 0.3s ease;
}

/* Pseudo-elements for holographic effects */
.holo-card::before,
.holo-card::after {
	content: "";
	position: absolute;
	left: 0;
	right: 0;
	top: 0;
	bottom: 0;
	background-position: 0% 0%;
	background-repeat: no-repeat;
	background-size: 300% 300%;
	mix-blend-mode: color-dodge;
	opacity: 0.2;
	z-index: 1;
	background-image: linear-gradient(
		115deg,
		transparent 0%,
		#54a29e 25%,
		transparent 47%,
		transparent 53%,
		#a79d66 75%,
		transparent 100%
	);
}

/* Sparkles effect */
.holo-card--sparkles::after {
	background-image:
		url("https://assets.codepen.io/13471/sparkles.gif"),
		linear-gradient(
			125deg,
			rgba(255, 0, 132, 0.31) 15%,
			rgba(252, 164, 0, 0.25) 30%,
			rgba(255, 255, 0, 0.19) 40%,
			rgba(0, 255, 138, 0.13) 60%,
			rgba(0, 207, 255, 0.25) 70%,
			rgba(204, 76, 250, 0.31) 85%
		);
	background-position: center;
	background-size: 180%;
	mix-blend-mode: color-dodge;
	opacity: 1;
	z-index: 1;
}

/* Active/hover state */
.holo-card--active::before {
	opacity: 1;
	animation: none;
	transition: none;
	background-image: linear-gradient(
		110deg,
		transparent 25%,
		#54a29e 48%,
		#a79d66 52%,
		transparent 75%
	);
}

/* Animated state (default) */
.holo-card--animated {
	transition: transform 1s ease;
}

.holo-card--animated::before {
	transition: all 1s ease;
	animation: holoGradient 12s ease infinite;
}

.holo-card--animated::after {
	transition: all 1s ease;
	animation: holoSparkle 12s ease infinite;
}

/* Card flip container */
.holo-card-container {
	perspective: 1000px;
	cursor: pointer;
}

.holo-card-container--static {
	perspective: none;
	cursor: default;
}

.holo-card-inner {
	position: relative;
	transition: transform 0.7s ease;
	transform-style: preserve-3d;
}

.holo-card-inner--flipped {
	transform: rotateY(180deg);
}

.holo-card-face {
	position: absolute;
	inset: 0;
	backface-visibility: hidden;
	-webkit-backface-visibility: hidden;
}

.holo-card-face--back {
	transform: rotateY(180deg);
}

/* Light mode adjustments */
:root[data-theme="light"] .holo-card,
.light .holo-card {
	box-shadow:
		-3px -3px 3px 0 rgba(38, 230, 247, 0.2),
		3px 3px 3px 0 rgba(247, 89, 228, 0.2),
		0 0 6px 2px rgba(255, 231, 89, 0.2),
		0 35px 25px -15px rgba(0, 0, 0, 0.15);
}
```

---

### iPhone Mockup <a name="iphone-mockup"></a>

A pixel-perfect iPhone Pro mockup with Dynamic Island, side buttons, status bar and home indicator.

- **Registry Key**: `iphone-mockup`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/iphone-mockup.json
```

#### How to Use

```tsx
import { IPhoneMockup } from "@/components/ui/iphone-mockup";

export default function Example() {
  return (
    <IPhoneMockup time="9:41">
      <div className="flex flex-1 items-center justify-center">
        Your app here
      </div>
    </IPhoneMockup>
  );
}
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| `time` | `string` | `"9:41"` | Status bar clock value. |
| `screenBackground` | `string` | `"#ffffff"` | CSS color for the screen background; used to auto-tint the status bar. |
| `statusBarTone` | `"auto" \| "light" \| "dark"` | `"auto"` | Status bar icon/text tint. |
| `hideStatusBar` | `boolean` | `false` | Hide the built-in status bar (let your app draw its own). |
| `homeIndicator` | `boolean` | `true` | Show the bottom home indicator bar. |
| `homeIndicatorTone` | `"auto" \| "light" \| "dark"` | `"auto"` | Tint for the home indicator. |
| `className` | `string` | – | Extra classes for the outer chassis. |
| `screenClassName` | `string` | – | Extra classes for the inner screen. |
| `children` | `React.ReactNode` | – | App content rendered inside the screen. |

---

### Knowledge Graph <a name="knowledge-graph"></a>

An interactive D3.js-powered knowledge graph visualization with zoom, pan, drag, tooltips, and export functionality.

- **Registry Key**: `knowledge-graph`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/knowledge-graph.json
```

#### Dependencies

- **NPM Packages**:
  - `d3`

#### How to Use

##### Example 1

```tsx
import { KnowledgeGraph } from "@/components/ui/knowledge-graph";

const nodes = [
  { id: "1", label: "React", type: "technology" },
  { id: "2", label: "TypeScript", type: "technology" },
  { id: "3", label: "User", type: "user", size: 30 },
];

const links = [
  { source: "3", target: "1", label: "uses" },
  { source: "3", target: "2", label: "uses" },
  { source: "1", target: "2" },
];

export default function Example() {
  return (
    <div className="h-[400px]">
      <KnowledgeGraph
        nodes={nodes}
        links={links}
        onNodeClick={(node) => console.log(node)}
      />
    </div>
  );
}
```

##### Example 2

```tsx
import { useRef } from "react";
import { KnowledgeGraph, type KnowledgeGraphHandle } from "@/components/ui/knowledge-graph";

export default function Example() {
  const graphRef = useRef<KnowledgeGraphHandle>(null);

  return (
    <div>
      <button onClick={() => graphRef.current?.exportAsPNG()}>
        Export as PNG
      </button>
      <button onClick={() => graphRef.current?.resetZoom()}>
        Reset Zoom
      </button>
      <KnowledgeGraph ref={graphRef} nodes={nodes} links={links} />
    </div>
  );
}
```

##### Example 3

```tsx
<KnowledgeGraph
  nodes={nodes}
  links={links}
  showLegend={false}
  showLinkLabels={false}
/>
```

##### Example 4

```tsx
const nodes = [
  { id: "1", label: "Important", type: "main", size: 40, color: "#ff0000" },
  { id: "2", label: "Related", type: "secondary", size: 20 },
];
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `nodes` | `GraphNode[]` | Required | Array of nodes to display |
| `links` | `GraphLink[]` | Required | Array of links connecting nodes |
| `onNodeClick` | `(node: GraphNode) => void` | - | Callback when node is clicked |
| `onNodeHover` | `(node: GraphNode \| null) => void` | - | Callback on node hover |
| `showLegend` | `boolean` | `true` | Show node type legend |
| `showLinkLabels` | `boolean` | `true` | Show relationship labels on links |
| `centerNodeId` | `string` | - | Node ID to position in center |
| `className` | `string` | - | Additional CSS classes |

#### Component Source Code

##### File: `registry/new-york/ui/knowledge-graph.tsx`

```tsx
"use client";

import {
	forwardRef,
	useCallback,
	useEffect,
	useImperativeHandle,
	useRef,
	useState,
} from "react";
import * as d3 from "d3";
import { cn } from "@/lib/utils";

// ============================================================================
// Types
// ============================================================================

export interface GraphNode extends d3.SimulationNodeDatum {
	/** Unique identifier for the node */
	id: string;
	/** Display label for the node */
	label: string;
	/** Node type/category (used for coloring) */
	type: string;
	/** Optional node size (default: 20) */
	size?: number;
	/** Optional custom color */
	color?: string;
	/** Optional additional data */
	data?: unknown;
}

export interface GraphLink extends d3.SimulationLinkDatum<GraphNode> {
	/** Source node ID or node reference */
	source: string | GraphNode;
	/** Target node ID or node reference */
	target: string | GraphNode;
	/** Optional relationship label */
	label?: string;
	/** Optional link strength (default: 1) */
	strength?: number;
}

export interface KnowledgeGraphProps {
	/** Array of nodes to display */
	nodes: GraphNode[];
	/** Array of links connecting nodes */
	links: GraphLink[];
	/** Callback when a node is clicked */
	onNodeClick?: (node: GraphNode) => void;
	/** Callback when a node is hovered */
	onNodeHover?: (node: GraphNode | null) => void;
	/** Show legend for node types */
	showLegend?: boolean;
	/** Show link labels */
	showLinkLabels?: boolean;
	/** Center node ID (will be positioned in center) */
	centerNodeId?: string;
	/** Additional CSS classes */
	className?: string;
}

export interface KnowledgeGraphHandle {
	/** Export the graph as SVG */
	exportAsSVG: () => void;
	/** Export the graph as PNG */
	exportAsPNG: () => void;
	/** Reset zoom to default */
	resetZoom: () => void;
}

// ============================================================================
// Utility Functions
// ============================================================================

/** Generate a deterministic color from a string */
const stringToColor = (str: string): string => {
	let hash = 0;
	for (let i = 0; i < str.length; i++) {
		const char = str.charCodeAt(i);
		hash = (hash << 5) - hash + char;
		hash = hash & hash;
	}
	const hue = Math.abs(hash) % 360;
	const saturation = 65 + (Math.abs(hash) % 20);
	const lightness = 50 + (Math.abs(hash) % 15);
	return `hsl(${hue}, ${saturation}%, ${lightness}%)`;
};

/** Format type string for display */
const formatTypeLabel = (type: string): string =>
	type.replace(/_/g, " ").replace(/\b\w/g, (l) => l.toUpperCase());

// ============================================================================
// Component
// ============================================================================

export const KnowledgeGraph = forwardRef<
	KnowledgeGraphHandle,
	KnowledgeGraphProps
>(
	(
		{
			nodes,
			links,
			onNodeClick,
			onNodeHover,
			showLegend = true,
			showLinkLabels = true,
			centerNodeId,
			className = "",
		},
		ref,
	) => {
		const svgRef = useRef<SVGSVGElement>(null);
		const containerRef = useRef<HTMLDivElement>(null);
		const zoomRef = useRef<d3.ZoomBehavior<SVGSVGElement, unknown> | null>(
			null,
		);

		const [tooltip, setTooltip] = useState<{
			x: number;
			y: number;
			content: string;
			visible: boolean;
		}>({
			x: 0,
			y: 0,
			content: "",
			visible: false,
		});

		const [legendItems, setLegendItems] = useState<
			Array<{ type: string; color: string }>
		>([]);

		// Export as SVG
		const exportAsSVG = useCallback(() => {
			if (!svgRef.current) return;

			const clonedSvg = svgRef.current.cloneNode(true) as SVGSVGElement;
			const graphGroup = clonedSvg.querySelector("g");

			if (graphGroup) {
				const originalGroup = svgRef.current.querySelector("g") as SVGGElement;
				if (originalGroup) {
					const bbox = originalGroup.getBBox();
					const padding = 50;
					clonedSvg.setAttribute(
						"viewBox",
						`${bbox.x - padding} ${bbox.y - padding} ${bbox.width + padding * 2} ${bbox.height + padding * 2}`,
					);
					clonedSvg.setAttribute("width", `${bbox.width + padding * 2}`);
					clonedSvg.setAttribute("height", `${bbox.height + padding * 2}`);
					graphGroup.removeAttribute("transform");
				}
			}

			const svgData = new XMLSerializer().serializeToString(clonedSvg);
			const blob = new Blob([svgData], { type: "image/svg+xml;charset=utf-8" });
			const url = URL.createObjectURL(blob);
			const link = document.createElement("a");
			link.href = url;
			link.download = "knowledge-graph.svg";
			link.click();
			URL.revokeObjectURL(url);
		}, []);

		// Export as PNG
		const exportAsPNG = useCallback(() => {
			if (!svgRef.current) return;

			const clonedSvg = svgRef.current.cloneNode(true) as SVGSVGElement;
			const graphGroup = clonedSvg.querySelector("g");
			let bbox = { x: 0, y: 0, width: 800, height: 600 };

			const originalGroup = svgRef.current.querySelector("g") as SVGGElement;
			if (originalGroup) {
				bbox = originalGroup.getBBox();
			}

			const padding = 100;
			const width = bbox.width + padding * 2;
			const height = bbox.height + padding * 2;

			clonedSvg.setAttribute(
				"viewBox",
				`${bbox.x - padding} ${bbox.y - padding} ${width} ${height}`,
			);
			clonedSvg.setAttribute("width", `${width}`);
			clonedSvg.setAttribute("height", `${height}`);
			if (graphGroup) graphGroup.removeAttribute("transform");

			const canvas = document.createElement("canvas");
			const ctx = canvas.getContext("2d");
			const img = new window.Image();
			const scale = 2;

			canvas.width = width * scale;
			canvas.height = height * scale;
			ctx?.scale(scale, scale);

			if (ctx) {
				ctx.fillStyle = "#18181b";
				ctx.fillRect(0, 0, width, height);
			}

			img.onload = () => {
				ctx?.drawImage(img, 0, 0, width, height);
				canvas.toBlob((blob) => {
					if (blob) {
						const url = URL.createObjectURL(blob);
						const link = document.createElement("a");
						link.href = url;
						link.download = "knowledge-graph.png";
						link.click();
						URL.revokeObjectURL(url);
					}
				}, "image/png");
			};

			const svgData = new XMLSerializer().serializeToString(clonedSvg);
			const blob = new Blob([svgData], { type: "image/svg+xml;charset=utf-8" });
			img.src = URL.createObjectURL(blob);
		}, []);

		// Reset zoom
		const resetZoom = useCallback(() => {
			if (svgRef.current && zoomRef.current) {
				d3.select(svgRef.current)
					.transition()
					.duration(500)
					.call(zoomRef.current.transform, d3.zoomIdentity);
			}
		}, []);

		useImperativeHandle(ref, () => ({
			exportAsSVG,
			exportAsPNG,
			resetZoom,
		}));

		// Main D3 effect
		useEffect(() => {
			if (!svgRef.current || !containerRef.current || nodes.length === 0)
				return;

			const container = containerRef.current;
			const svg = d3.select(svgRef.current);

			// Clear previous content
			svg.selectAll("*").remove();

			const width = container.clientWidth;
			const height = container.clientHeight;

			// Get unique node types for coloring
			const nodeTypes = [...new Set(nodes.map((n) => n.type))].sort();
			const colorMapping: Record<string, string> = {};
			for (const type of nodeTypes) {
				colorMapping[type] = stringToColor(type);
			}

			const colorScale = (type: string, customColor?: string) =>
				customColor || colorMapping[type] || "#6b7280";

			// Set legend items
			setLegendItems(
				nodeTypes.map((type) => ({
					type: formatTypeLabel(type),
					color: colorMapping[type],
				})),
			);

			// Clone data to avoid mutation
			const nodesCopy: GraphNode[] = nodes.map((n) => ({
				...n,
				size: n.size || 20,
			}));
			const linksCopy: GraphLink[] = links.map((l) => ({ ...l }));

			// Position center node if specified
			const centerNode = centerNodeId
				? nodesCopy.find((n) => n.id === centerNodeId)
				: null;
			if (centerNode) {
				centerNode.x = width / 2;
				centerNode.y = height / 2;
			}

			// Create simulation
			const simulation = d3
				.forceSimulation<GraphNode>(nodesCopy)
				.force(
					"link",
					d3
						.forceLink<GraphNode, GraphLink>(linksCopy)
						.id((d: GraphNode) => d.id)
						.distance(150)
						.strength((d: GraphLink) => d.strength || 0.8),
				)
				.force("charge", d3.forceManyBody().strength(-800))
				.force("center", d3.forceCenter(width / 2, height / 2))
				.force(
					"collision",
					d3
						.forceCollide<GraphNode>()
						.radius((d: GraphNode) => (d.size || 20) + 20),
				);

			// Create zoom behavior
			const zoom = d3
				.zoom<SVGSVGElement, unknown>()
				.scaleExtent([0.1, 4])
				.on("zoom", (event: d3.D3ZoomEvent<SVGSVGElement, unknown>) => {
					g.attr("transform", event.transform.toString());
				});

			zoomRef.current = zoom;
			svg.call(zoom);

			// Create container group
			const g = svg.append("g");

			// Create links
			const link = g
				.selectAll<SVGLineElement, GraphLink>(".link")
				.data(linksCopy)
				.join("line")
				.attr("class", "link")
				.attr("stroke", "#6b7280")
				.attr("stroke-opacity", 0.5)
				.attr("stroke-width", 2);

			// Create link labels
			const linkLabel = showLinkLabels
				? g
						.selectAll<SVGTextElement, GraphLink>(".link-label")
						.data(linksCopy.filter((l) => l.label))
						.join("text")
						.attr("class", "link-label")
						.attr("text-anchor", "middle")
						.attr("font-size", "10px")
						.attr("fill", "#9ca3af")
						.text((d: GraphLink) => d.label || "")
				: null;

			// Create node groups
			const nodeGroup = g
				.selectAll<SVGGElement, GraphNode>(".node-group")
				.data(nodesCopy)
				.join("g")
				.attr("class", "node-group")
				.style("cursor", "pointer")
				.call(
					d3
						.drag<SVGGElement, GraphNode>()
						.on(
							"start",
							(
								event: d3.D3DragEvent<SVGGElement, GraphNode, GraphNode>,
								d: GraphNode,
							) => {
								if (!event.active) simulation.alphaTarget(0.3).restart();
								d.fx = d.x;
								d.fy = d.y;
							},
						)
						.on(
							"drag",
							(
								event: d3.D3DragEvent<SVGGElement, GraphNode, GraphNode>,
								d: GraphNode,
							) => {
								d.fx = event.x;
								d.fy = event.y;
							},
						)
						.on(
							"end",
							(
								event: d3.D3DragEvent<SVGGElement, GraphNode, GraphNode>,
								d: GraphNode,
							) => {
								if (!event.active) simulation.alphaTarget(0);
								d.fx = null;
								d.fy = null;
							},
						),
				);

			// Add circles
			nodeGroup
				.append("circle")
				.attr("r", (d: GraphNode) => d.size || 20)
				.attr("fill", (d: GraphNode) => colorScale(d.type, d.color))
				.attr("stroke", "#27272a")
				.attr("stroke-width", 2);

			// Add labels
			nodeGroup
				.append("text")
				.attr("class", "node-label")
				.attr("text-anchor", "middle")
				.attr("dy", ".35em")
				.attr("font-size", "11px")
				.attr("font-weight", "500")
				.attr("fill", "#ffffff")
				.attr("pointer-events", "none")
				.text((d: GraphNode) =>
					d.label.length > 12 ? `${d.label.substring(0, 10)}...` : d.label,
				);

			// Add event handlers
			nodeGroup
				.on("click", (_event: MouseEvent, d: GraphNode) => {
					onNodeClick?.(d);
				})
				.on("mouseover", (event: MouseEvent, d: GraphNode) => {
					const [x, y] = d3.pointer(event, container);
					setTooltip({
						x: x + 10,
						y: y - 10,
						content: `${d.label} (${formatTypeLabel(d.type)})`,
						visible: true,
					});
					onNodeHover?.(d);
				})
				.on("mouseout", () => {
					setTooltip((prev) => ({ ...prev, visible: false }));
					onNodeHover?.(null);
				});

			// Update positions on tick
			simulation.on("tick", () => {
				link
					.attr("x1", (d: GraphLink) => (d.source as GraphNode).x ?? 0)
					.attr("y1", (d: GraphLink) => (d.source as GraphNode).y ?? 0)
					.attr("x2", (d: GraphLink) => (d.target as GraphNode).x ?? 0)
					.attr("y2", (d: GraphLink) => (d.target as GraphNode).y ?? 0);

				if (linkLabel) {
					linkLabel
						.attr(
							"x",
							(d: GraphLink) =>
								(((d.source as GraphNode).x ?? 0) +
									((d.target as GraphNode).x ?? 0)) /
								2,
						)
						.attr(
							"y",
							(d: GraphLink) =>
								(((d.source as GraphNode).y ?? 0) +
									((d.target as GraphNode).y ?? 0)) /
								2,
						);
				}

				nodeGroup.attr(
					"transform",
					(d: GraphNode) => `translate(${d.x ?? 0},${d.y ?? 0})`,
				);
			});

			// Cleanup
			return () => {
				simulation.stop();
			};
		}, [nodes, links, onNodeClick, onNodeHover, showLinkLabels, centerNodeId]);

		return (
			<div className={cn("relative h-full w-full min-h-[400px]", className)}>
				<div ref={containerRef} className="h-full w-full">
					<svg
						ref={svgRef}
						width="100%"
						height="100%"
						className="bg-zinc-50 dark:bg-zinc-900 rounded-xl"
					/>
				</div>

				{/* Tooltip */}
				{tooltip.visible && (
					<div
						className="pointer-events-none absolute z-10 px-3 py-2 text-sm rounded-lg bg-zinc-800 text-zinc-100 shadow-lg border border-zinc-700"
						style={{ left: tooltip.x, top: tooltip.y }}
					>
						{tooltip.content}
					</div>
				)}

				{/* Legend */}
				{showLegend && legendItems.length > 0 && (
					<div className="absolute top-4 right-4 z-10 p-3 rounded-lg bg-zinc-800/90 backdrop-blur-sm border border-zinc-700">
						<div className="max-h-48 space-y-1.5 overflow-y-auto">
							{legendItems.map((item) => (
								<div key={item.type} className="flex items-center gap-2">
									<div
										className="h-3 w-3 rounded-full flex-shrink-0"
										style={{ backgroundColor: item.color }}
									/>
									<span className="text-xs text-zinc-300">{item.type}</span>
								</div>
							))}
						</div>
					</div>
				)}
			</div>
		);
	},
);

KnowledgeGraph.displayName = "KnowledgeGraph";

export default KnowledgeGraph;
```

---

### Line Chart <a name="line-chart"></a>

A line chart for trends over time with optional dots, labels, and multiple series.

- **Registry Key**: `line-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/line-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `chart`
  - `card`

#### How to Use

```tsx
import { LineChart } from "@/components/ui/line-chart";

const data = [
  { day: "Mon", visitors: 320, signups: 24 },
  { day: "Tue", visitors: 410, signups: 31 },
  { day: "Wed", visitors: 380, signups: 28 },
];

export default function Example() {
  return (
    <LineChart
      data={data}
      xKey="day"
      yKeys={["visitors", "signups"]}
      title="Weekly Activity"
    />
  );
}
```

#### Props & API Reference

##### `LineChart` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| data | Array&lt;Record&lt;string, string \| number&gt;&gt; | - | Chart data rows |
| xKey | string | - | Key on each row used for the X axis |
| yKeys | string \| string[] | - | One or more numeric keys to plot |
| title | string | - | Card title |
| description | string | - | Card description |
| footer | string | - | Optional footer text |
| colors | string[] | CHART_COLORS | Custom palette for series |
| showDots | boolean | true | Render point markers on each line |
| showLabels | boolean | false | Render inline labels above points |

#### Component Source Code

##### File: `registry/new-york/ui/line-chart.tsx`

```tsx
"use client";

import type * as React from "react";
import {
	LabelList,
	Line,
	LineChart as RechartsLineChart,
	XAxis,
	YAxis,
} from "recharts";
import { Card, CardContent, CardFooter, CardHeader } from "@/components/ui/card";
import {
	type ChartConfig,
	ChartContainer,
	ChartLegend,
	ChartLegendContent,
	ChartTooltip,
	ChartTooltipContent,
} from "@/components/ui/chart";

const CHART_COLORS = ["#00bbff", "#34d399", "#60a5fa", "#f472b6", "#fb923c"];

function toKeys(v: string | string[]): string[] {
	return Array.isArray(v) ? v : [v];
}

function ChartCard({
	title,
	description,
	footer,
	children,
	dataPoints = 0,
}: {
	title?: string;
	description?: string;
	footer?: string;
	children: React.ReactNode;
	dataPoints?: number;
}) {
	const width =
		dataPoints > 0 ? Math.min(768, Math.max(300, dataPoints * 80)) : undefined;
	return (
		<Card
			className="max-w-3xl"
			style={width ? { width } : { width: "100%" }}
		>
			{(title || description) && (
				<CardHeader className="pb-0">
					{title && (
						<p className="text-sm font-semibold text-card-foreground">
							{title}
						</p>
					)}
					{description && (
						<p className="text-xs text-muted-foreground">{description}</p>
					)}
				</CardHeader>
			)}
			<CardContent>{children}</CardContent>
			{footer && (
				<CardFooter>
					<p className="text-xs text-muted-foreground">{footer}</p>
				</CardFooter>
			)}
		</Card>
	);
}

export type LineChartProps = {
	data: Array<Record<string, string | number>>;
	xKey: string;
	yKeys: string | string[];
	title?: string;
	description?: string;
	footer?: string;
	colors?: string[];
	showDots?: boolean;
	showLabels?: boolean;
};

export function LineChart(props: LineChartProps) {
	const keys = toKeys(props.yKeys);
	const colors = props.colors ?? CHART_COLORS;
	const chartConfig: ChartConfig = Object.fromEntries(
		keys.map((key, i) => [
			key,
			{ label: key, color: colors[i % colors.length] },
		]),
	);
	const showLegend = keys.length > 1;
	return (
		<ChartCard
			title={props.title}
			description={props.description}
			footer={props.footer}
			dataPoints={props.data.length}
		>
			<ChartContainer config={chartConfig} className="h-50 w-full">
				<RechartsLineChart
					data={props.data}
					{...(props.showLabels === true
						? { margin: { top: 24, left: 12, right: 12 } }
						: {})}
				>
					<XAxis
						dataKey={props.xKey}
						axisLine={false}
						tickLine={false}
						tick={{ fill: "#71717a", fontSize: 11 }}
					/>
					<YAxis
						axisLine={false}
						tickLine={false}
						tick={{ fill: "#71717a", fontSize: 11 }}
					/>
					<ChartTooltip content={<ChartTooltipContent />} />
					{showLegend && <ChartLegend content={<ChartLegendContent />} />}
					{keys.map((key) => (
						<Line
							key={key}
							type="monotone"
							dataKey={key}
							stroke={`var(--color-${key})`}
							strokeWidth={2}
							dot={
								props.showDots !== false
									? { fill: `var(--color-${key})` }
									: false
							}
						>
							{props.showLabels === true && (
								<LabelList
									position="top"
									offset={12}
									fontSize={12}
									fill="#a1a1aa"
									dataKey={props.xKey}
								/>
							)}
						</Line>
					))}
				</RechartsLineChart>
			</ChartContainer>
		</ChartCard>
	);
}
```

---

### Link Preview <a name="link-preview"></a>

An interactive link component with rich preview tooltip showing website metadata like title, description, and images.

- **Registry Key**: `link-preview`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/link-preview.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `icons`

#### How to Use

```tsx
import { LinkPreview } from "@/components/ui/link-preview";

export default function Example() {
  return (
    <p className="text-zinc-300">
      Check out the{" "}
      <LinkPreview href="https://github.com/theexperiencecompany/gaia">
        GAIA repository
      </LinkPreview>{" "}
      to learn more about this open-source AI assistant.
    </p>
  );
}
```

#### Props & API Reference

##### `Props` Props

| Prop      | Type                | Default                                      | Description                     |
| --------- | ------------------- | -------------------------------------------- | ------------------------------- |
| href      | string              | -                                            | The URL to preview              |
| children  | ReactNode \| string | -                                            | Link text content               |
| className | string              | "cursor-pointer rounded-sm bg-primary/20..." | Custom CSS classes for the link |

#### Component Source Code

##### File: `registry/new-york/ui/link-preview.tsx`

```tsx
"use client";

import Image from "next/image";
import { type ReactNode, useEffect, useRef, useState } from "react";
import { Globe02Icon, HugeiconsIcon } from "@/components/icons";

import {
	Tooltip,
	TooltipContent,
	TooltipTrigger,
} from "@/components/ui/tooltip";

interface UrlMetadata {
	title: string | null;
	description: string | null;
	favicon: string | null;
	website_name: string | null;
	website_image: string | null;
	url: string;
}

interface LinkPreviewProps {
	href: string;
	children: ReactNode | string | null;
	className?: string;
}

const isEmail = (str: string) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(str);

const isValidHttpUrl = (str: string): boolean => {
	try {
		const url = new URL(str);
		return /^(http|https):$/.test(url.protocol);
	} catch {
		return false;
	}
};

export function LinkPreview({
	href,
	children,
	className = "cursor-pointer rounded-sm bg-primary/20 px-1 text-sm font-medium text-primary transition-all hover:text-white hover:underline",
}: LinkPreviewProps) {
	const elementRef = useRef<HTMLAnchorElement>(null);
	const [isInView, setIsInView] = useState(false);
	const [validFavicon, setValidFavicon] = useState(true);
	const [validImage, setValidImage] = useState(true);
	const [metadata, setMetadata] = useState<UrlMetadata | null>(null);
	const [isLoading, setIsLoading] = useState(false);
	const [error, setError] = useState<Error | null>(null);

	const isValidUrl =
		href &&
		isValidHttpUrl(href) &&
		!isEmail(href) &&
		!href.startsWith("mailto:");

	// Fetch metadata when in view
	useEffect(() => {
		if (!isInView || !isValidUrl || metadata) return;

		let isMounted = true;

		async function fetchMetadata() {
			setIsLoading(true);
			setError(null);

			try {
				// Fetch the URL directly
				const response = await fetch(href);

				if (!response.ok) {
					throw new Error("Failed to fetch URL");
				}

				const html = await response.text();

				// Extract metadata from HTML
				const getMetaTag = (name: string): string | null => {
					const patterns = [
						new RegExp(
							`<meta[^>]*property=["']${name}["'][^>]*content=["']([^"']*)["']`,
							"i",
						),
						new RegExp(
							`<meta[^>]*name=["']${name}["'][^>]*content=["']([^"']*)["']`,
							"i",
						),
						new RegExp(
							`<meta[^>]*content=["']([^"']*)["'][^>]*property=["']${name}["']`,
							"i",
						),
						new RegExp(
							`<meta[^>]*content=["']([^"']*)["'][^>]*name=["']${name}["']`,
							"i",
						),
					];

					for (const pattern of patterns) {
						const match = html.match(pattern);
						if (match) return match[1];
					}
					return null;
				};

				const titleMatch = html.match(/<title[^>]*>([^<]+)<\/title>/i);
				const urlObj = new URL(href);

				const data: UrlMetadata = {
					title:
						getMetaTag("og:title") ||
						getMetaTag("twitter:title") ||
						titleMatch?.[1] ||
						null,
					description:
						getMetaTag("og:description") ||
						getMetaTag("twitter:description") ||
						getMetaTag("description") ||
						null,
					website_image:
						getMetaTag("og:image") || getMetaTag("twitter:image") || null,
					favicon:
						getMetaTag("icon") ||
						getMetaTag("shortcut icon") ||
						`${urlObj.origin}/favicon.ico`,
					website_name: getMetaTag("og:site_name") || urlObj.hostname,
					url: href,
				};

				if (isMounted) {
					setMetadata(data);
					setIsLoading(false);
				}
			} catch (err) {
				if (isMounted) {
					setError(err as Error);
					setIsLoading(false);
				}
			}
		}

		fetchMetadata();

		return () => {
			isMounted = false;
		};
	}, [isInView, isValidUrl, href, metadata]);

	// Set up intersection observer to detect when element is in view
	useEffect(() => {
		const element = elementRef.current;
		if (!element || !href) return;

		const observer = new IntersectionObserver(
			([entry]) => {
				if (entry.isIntersecting) {
					setIsInView(true);
					observer.unobserve(element);
				}
			},
			{
				rootMargin: "100px", // Start fetching 100px before element comes into view
				threshold: 0.1,
			},
		);

		observer.observe(element);

		return () => {
			observer.unobserve(element);
		};
	}, [href]);

	if (!href) return null;

	return (
		<Tooltip>
			<TooltipTrigger asChild>
				<a
					ref={elementRef}
					href={href}
					className={className}
					rel="noopener noreferrer"
					target="_blank"
				>
					{children}
				</a>
			</TooltipTrigger>
			<TooltipContent className="max-w-[280px] border border-zinc-700 bg-zinc-900 p-3 text-white shadow-lg">
				{isLoading ? (
					<div className="flex justify-center p-5">
						<div className="size-5 animate-spin rounded-full border-2 border-zinc-700 border-t-white" />
					</div>
				) : error || !isValidUrl ? (
					<div className="flex items-center gap-2 p-3 text-red-400">
						<HugeiconsIcon icon={Globe02Icon} size={16} />
						<span className="text-sm">
							{!isValidUrl ? "Invalid URL" : "Failed to load preview"}
						</span>
					</div>
				) : metadata ? (
					<div className="flex w-full flex-col gap-2">
						{/* Website Image */}
						{metadata.website_image && validImage && (
							<div className="relative aspect-video w-full overflow-hidden rounded-lg">
								<Image
									src={metadata.website_image}
									alt="Website preview"
									fill
									className="rounded-lg object-cover"
									onError={() => setValidImage(false)}
								/>
							</div>
						)}

						{/* Website Name & Favicon */}
						{(metadata.website_name || (metadata.favicon && validFavicon)) && (
							<div className="flex items-center gap-2">
								{metadata.favicon && validFavicon ? (
									<Image
										width={20}
										height={20}
										alt="Favicon"
										className="size-5 rounded-full"
										src={metadata.favicon}
										onError={() => setValidFavicon(false)}
									/>
								) : (
									<HugeiconsIcon
										icon={Globe02Icon}
										size={20}
										className="text-gray-400"
									/>
								)}
								{metadata.website_name && (
									<div className="truncate text-sm font-semibold">
										{metadata.website_name}
									</div>
								)}
							</div>
						)}

						{/* Title */}
						{metadata.title && (
							<div className="truncate text-sm font-medium text-white">
								{metadata.title}
							</div>
						)}

						{/* Description */}
						{metadata.description && (
							<div className="line-clamp-3 text-xs text-gray-400 w-full">
								{metadata.description}
							</div>
						)}

						{/* URL Link */}
						<div className="truncate text-xs text-primary">
							{href.replace("https://", "").replace("http://", "")}
						</div>
					</div>
				) : (
					<div className="flex items-center gap-2 p-3">
						<HugeiconsIcon
							icon={Globe02Icon}
							size={16}
							className="text-gray-400"
						/>
						<span className="text-sm text-gray-400">No preview available</span>
					</div>
				)}
			</TooltipContent>
		</Tooltip>
	);
}
```

---

### iOS Message Bubbles <a name="message-bubble"></a>

Beautiful iOS-style message bubbles with authentic iMessage appearance, message grouping, and tail effects.

- **Registry Key**: `message-bubble`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/message-bubble.json
```

#### How to Use

##### Example 1

```tsx
import { MessageBubble } from "@/components/ui/message-bubble";

export default function Example() {
  return (
    <>
      <MessageBubble message="Hey there!" variant="received" />
      <MessageBubble message="Hi! How are you?" variant="sent" />
    </>
  );
}
```

##### Example 2

```tsx
import { ChatMessage } from "@/components/ui/message-bubble";

export default function Example() {
  return (
    <ChatMessage
      variant="received"
      messages={["Hey!", "How are you doing?", "Long time no see!"]}
      timestamp="10:30 AM"
    />
  );
}
```

##### Example 3

```tsx
<MessageBubble variant="received">
  <div className="flex items-center gap-2">
    <img src="/emoji.png" className="size-6" />
    <span>Custom content with emoji!</span>
  </div>
</MessageBubble>
```

##### Example 4

```tsx
<MessageBubble message="First message" variant="sent" grouped="first" />
<MessageBubble message="Middle message" variant="sent" grouped="middle" />
<MessageBubble message="Last message" variant="sent" grouped="last" />
```

##### Example 5

```tsx
<MessageBubble
  message="This is a really long message that will automatically wrap to multiple lines while maintaining the iOS-style bubble appearance with proper padding and borders."
  variant="received"
/>
```

#### Props & API Reference

##### `MessageBubble` Props

| Prop      | Type                                    | Default    | Description                        |
| --------- | --------------------------------------- | ---------- | ---------------------------------- |
| message   | string                                  | -          | The message text to display        |
| variant   | "sent" \| "received"                    | "received" | Message alignment and styling      |
| grouped   | "first" \| "middle" \| "last" \| "none" | "none"     | Position in grouped messages       |
| className | string                                  | undefined  | Additional CSS classes             |
| children  | ReactNode                               | undefined  | Custom content (overrides message) |

##### `ChatMessage` Props

| Prop          | Type                 | Default    | Description                   |
| ------------- | -------------------- | ---------- | ----------------------------- |
| messages      | string[]             | -          | Array of message texts        |
| variant       | "sent" \| "received" | "received" | Message alignment and styling |
| timestamp     | string               | undefined  | Message timestamp             |
| showTimestamp | boolean              | true       | Whether to show timestamp     |
| className     | string               | undefined  | Additional CSS classes        |

#### Component Source Code

##### File: `registry/new-york/ui/message-bubble.tsx`

```tsx
"use client";

import "./message-bubble.css";

import { cn } from "@/lib/utils";

export interface MessageBubbleProps {
	message: string;
	variant?: "sent" | "received";
	grouped?: "first" | "middle" | "last" | "none";
	className?: string;
	children?: React.ReactNode;
}

export function MessageBubble({
	message,
	variant = "received",
	grouped = "none",
	className,
	children,
}: MessageBubbleProps) {
	const groupedClasses =
		grouped === "first"
			? "imessage-grouped-first mb-1.5"
			: grouped === "last"
				? "imessage-grouped-last"
				: grouped === "middle"
					? "imessage-grouped-middle mb-1.5"
					: "";

	return (
		<div
			className={cn(
				"imessage-bubble",
				variant === "sent" ? "imessage-from-me" : "imessage-from-them",
				groupedClasses,
				className,
			)}
		>
			{children || <p className="whitespace-pre-wrap">{message}</p>}
		</div>
	);
}

export interface ChatMessageProps {
	timestamp?: string;
	messages: string[];
	variant?: "sent" | "received";
	className?: string;
	showTimestamp?: boolean;
}

export function ChatMessage({
	timestamp,
	messages,
	variant = "received",
	className,
	showTimestamp = true,
}: ChatMessageProps) {
	const hasMultipleMessages = messages.length > 1;

	const getGroupedType = (index: number, total: number) => {
		if (total === 1) return "none";
		if (index === 0) return "first";
		if (index === total - 1) return "last";
		return "middle";
	};

	return (
		<div className={cn("flex w-full flex-col", className)}>
			<div className="flex flex-col">
				{messages.map((message, index) => (
					<MessageBubble
						key={index}
						message={message}
						variant={variant}
						grouped={
							hasMultipleMessages
								? getGroupedType(index, messages.length)
								: "none"
						}
					/>
				))}
			</div>

			{showTimestamp && timestamp && (
				<span
					className={cn(
						"mt-1 px-2 text-xs text-muted-foreground",
						variant === "sent" && "text-right",
					)}
				>
					{timestamp}
				</span>
			)}
		</div>
	);
}
```

##### File: `registry/new-york/ui/message-bubble.css`

```tsx
/* iOS-style message bubbles */
.imessage-bubble {
	word-wrap: break-word;
	line-height: 24px;
	position: relative;
	padding: 8px 20px;
	max-width: 60vw;
	border-radius: 20px;
}

.imessage-bubble::before,
.imessage-bubble::after {
	content: "";
	position: absolute;
	bottom: 0;
	height: 18px;
}

/* Grouped messages have tighter corner radii on the stacking side */
/* Received messages (left side) - tight corners on left side */
.imessage-grouped-first.imessage-from-them {
	/* biome-ignore lint/complexity/noImportantStyles: need to override radius */
	border-radius: 20px 20px 20px 5px !important;
}

.imessage-grouped-middle.imessage-from-them {
	/* biome-ignore lint/complexity/noImportantStyles: need to override radius */
	border-radius: 5px 20px 20px 5px !important;
}

.imessage-grouped-last.imessage-from-them {
	/* biome-ignore lint/complexity/noImportantStyles: need to override radius */
	border-radius: 5px 20px 20px 20px !important;
}

.imessage-grouped-last.imessage-from-them {
	/* biome-ignore lint/complexity/noImportantStyles: need to override radius */
	border-radius: 5px 20px 20px 20px !important;
}

/* Sent messages (right side) - tight corners on right side */
.imessage-grouped-first.imessage-from-me {
	/* biome-ignore lint/complexity/noImportantStyles: need to override radius */
	border-radius: 20px 20px 5px 20px !important;
}

.imessage-grouped-middle.imessage-from-me {
	/* biome-ignore lint/complexity/noImportantStyles: need to override radius */
	border-radius: 20px 5px 5px 20px !important;
}

.imessage-grouped-last.imessage-from-me {
	/* biome-ignore lint/complexity/noImportantStyles: need to override radius */
	border-radius: 20px 5px 20px 20px !important;
}

/* Bot messages (from-them style) */
.imessage-from-them {
	background-color: var(--color-zinc-300);
	color: black;
	align-self: flex-start;
}

.imessage-from-them::before {
	left: -7px;
	width: 20px;
	background-color: var(--color-zinc-300);
	border-bottom-right-radius: 16px;
}

.imessage-from-them::after {
	left: -26px;
	width: 26px;
	background-color: var(--color-background);
	border-bottom-right-radius: 10px;
}

/* User messages (from-me style) */
.imessage-from-me {
	color: black;
	background: #00bbff;
	align-self: flex-end;
}

.imessage-from-me::before {
	right: -7px;
	width: 20px;
	background-color: #00bbff;
	border-bottom-left-radius: 16px 14px;
}

.imessage-from-me::after {
	right: -26px;
	width: 26px;
	background-color: var(--color-background);
	border-bottom-left-radius: 10px;
}

/* Grouped iMessage bubbles for NEW_MESSAGE_BREAK */
/* Hide tails for grouped messages except the last one */
.imessage-grouped-first::before,
.imessage-grouped-first::after,
.imessage-grouped-middle::before,
.imessage-grouped-middle::after {
	display: none;
}
```

---

### Model Selector <a name="model-selector"></a>

A dropdown selector for choosing AI models with provider information and pro badges.

- **Registry Key**: `model-selector`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/model-selector.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
import { ModelSelector } from "@/components/ui/model-selector";
import { useState } from "react";

const models = [
  { id: "gpt-4", name: "GPT-4", provider: "OpenAI", isPro: true },
  { id: "claude", name: "Claude", provider: "Anthropic" },
];

export default function Example() {
  const [selected, setSelected] = useState(models[0]);
  
  return (
    <ModelSelector
      models={models}
      selectedModel={selected}
      onSelect={setSelected}
    />
  );
}
```

#### Component Source Code

##### File: `registry/new-york/ui/model-selector.tsx`

```tsx

```

---

### Navbar Menu <a name="navbar-menu"></a>

A beautiful animated dropdown menu for navigation with support for icons, descriptions, and custom layouts.

- **Registry Key**: `navbar-menu`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/navbar-menu.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `next`
- **Local Registry Dependencies**:
  - `raised-button`

#### Props & API Reference

##### `NavbarMenu` Props

| Prop       | Type                | Default   | Description                                 |
| ---------- | ------------------- | --------- | ------------------------------------------- |
| activeMenu | string              | -         | The ID of the currently active menu section |
| sections   | NavbarMenuSection[] | -         | Array of menu sections with links           |
| onClose    | () => void          | undefined | Optional callback when menu should close    |

#### Component Source Code

##### File: `registry/new-york/ui/navbar-menu.tsx`

```tsx
"use client";

import { AnimatePresence, motion } from "motion/react";
import Image from "next/image";
import * as React from "react";
import { ArrowDown01Icon, HugeiconsIcon } from "@/components/icons";

import { cn } from "@/lib/utils";
import { RaisedButton } from "@/registry/new-york/ui/raised-button";

export interface NavbarMenuLink {
  label: string;
  href: string;
  icon?: React.ReactNode;
  external?: boolean;
  description?: string;
  backgroundImage?: string;
  rowSpan?: number;
}

export interface NavbarMenuSection {
  id: string;
  links: NavbarMenuLink[];
  gridLayout?: string;
}

export interface NavbarMenuProps {
  activeMenu: string;
  sections: NavbarMenuSection[];
  onClose?: () => void;
}

export interface NavbarWithMenuProps {
  sections: NavbarMenuSection[];
  navItems?: Array<
    | { type: "link"; label: string; href: string }
    | { type: "dropdown"; label: string; menu: string }
  >;
  logo?: React.ReactNode;
  cta?: React.ReactNode;
}

const ListItem = React.forwardRef<
  HTMLAnchorElement,
  React.AnchorHTMLAttributes<HTMLAnchorElement> & {
    title: string;
    children?: React.ReactNode;
    href: string;
    external?: boolean;
    icon?: React.ReactNode;
    backgroundImage?: string;
    rowSpan?: number;
  }
>(
  (
    {
      className,
      title,
      children,
      href,
      external,
      icon,
      backgroundImage,
      rowSpan,
      ...props
    },
    ref,
  ) => {
    return (
      <li className={cn("list-none", rowSpan === 2 && "row-span-2")}>
        <a
          ref={ref}
          href={href}
          target={external ? "_blank" : undefined}
          rel={external ? "noopener noreferrer" : undefined}
          className={cn(
            "group relative flex h-full min-h-18 w-full flex-col justify-center overflow-hidden rounded-2xl bg-zinc-800/0 p-3.5 leading-none no-underline outline-none transition-all duration-150 select-none hover:bg-zinc-800 hover:text-zinc-100 focus:bg-zinc-800 focus:text-zinc-100",
            className,
          )}
          {...props}
        >
          {backgroundImage && (
            <>
              <Image
                fill
                src={backgroundImage}
                alt={title}
                className="absolute inset-0 z-0 h-full w-full object-cover transition-all group-hover:brightness-60"
              />
              <div className="absolute inset-0 z-[1] bg-gradient-to-t from-black/90 via-black/50 to-black/20" />
            </>
          )}
          <div
            className={cn(
              "flex items-start gap-3",
              backgroundImage && "relative z-[2] mt-auto",
            )}
          >
            {icon && (
              <span
                className={cn(
                  "relative flex min-h-10 min-w-10 items-center justify-center rounded-xl p-2 text-primary transition group-hover:text-zinc-300",
                  backgroundImage
                    ? "bg-white/5 backdrop-blur group-hover:bg-white/10"
                    : "bg-zinc-800/80 group-hover:bg-zinc-700/80",
                )}
              >
                {icon}
              </span>
            )}
            <div className="flex h-full flex-col justify-start gap-1 leading-none font-normal text-zinc-100">
              {title}

              {children && (
                <p
                  className={cn(
                    "line-clamp-2 text-sm leading-tight font-light text-zinc-500",
                    backgroundImage && "relative z-[2]",
                  )}
                >
                  {children}
                </p>
              )}
            </div>
          </div>
        </a>
      </li>
    );
  },
);

ListItem.displayName = "ListItem";

export function NavbarMenu({ activeMenu, sections }: NavbarMenuProps) {
  const activeSection = sections.find((section) => section.id === activeMenu);

  if (!activeSection) return null;

  const gridLayout =
    activeSection.gridLayout || "grid w-full grid-cols-2 gap-4";

  return (
    <motion.div
      initial={{ scaleY: 0.95, opacity: 0 }}
      animate={{ scaleY: 1, opacity: 1 }}
      exit={{ scaleY: 0.95, opacity: 0 }}
      transition={{
        ease: [0.19, 1, 0.15, 1.01],
      }}
      className={cn(
        "absolute top-full left-0 z-40 w-full origin-top overflow-hidden rounded-b-2xl border-1 border-y-0 border-white/5 bg-gradient-to-b from-zinc-950 to-zinc-900/30 backdrop-blur-2xl outline-none",
      )}
    >
      <div className="p-6">
        <ul className={gridLayout}>
          {activeSection.links.map((link) => (
            <ListItem
              key={link.href}
              href={link.href}
              title={link.label}
              external={link.external}
              icon={link.icon}
              backgroundImage={link.backgroundImage}
              rowSpan={link.rowSpan}
            >
              {link.description}
            </ListItem>
          ))}
        </ul>
      </div>
    </motion.div>
  );
}

export function NavbarWithMenu({
  sections,
  navItems,
  logo,
  cta,
}: NavbarWithMenuProps) {
  const [activeDropdown, setActiveDropdown] = React.useState<string | null>(
    null,
  );
  const [hoveredItem, setHoveredItem] = React.useState<string | null>(null);

  const defaultNavItems = [
    { type: "dropdown", label: "Product", menu: "product" },
    { type: "dropdown", label: "Resources", menu: "resources" },
    { type: "dropdown", label: "Socials", menu: "socials" },
  ] as const;

  const items = navItems || defaultNavItems;

  const handleNavbarMouseLeave = () => {
    setActiveDropdown(null);
    setHoveredItem(null);
  };

  const handleMouseEnter = (menu: string) => {
    setActiveDropdown(menu);
    setHoveredItem(menu);
  };

  return (
    <div className="min-h-[350px] w-full bg-zinc-950 p-4 flex items-start justify-center transition">
      {/* biome-ignore lint/a11y/noStaticElementInteractions: Hover container for menu, not interactive content */}
      <div
        className="relative mx-auto w-screen max-w-4xl"
        onMouseLeave={handleNavbarMouseLeave}
      >
        <div
          className={cn(
            "navbar_content flex h-14 w-full items-center justify-between border border-white/5 px-3 backdrop-blur-md transition-all",
            activeDropdown
              ? "rounded-t-2xl border-b-0 bg-zinc-950"
              : "rounded-2xl bg-zinc-900/30",
          )}
        >
          <div className="flex items-center gap-2 px-2">
            {logo || (
              <Image
                src="/media/text_w_logo_white.webp"
                alt="Logo"
                width={100}
                height={30}
                className="object-contain"
              />
            )}
          </div>

          <div className="flex items-center gap-1 rounded-lg px-1 py-1">
            {items.map((item) =>
              item.type === "link" ? (
                <button
                  type="button"
                  key={item.href}
                  className={cn(
                    "relative flex h-9 cursor-pointer items-center rounded-xl px-4 py-2 text-sm transition-colors hover:bg-zinc-800/40",
                    hoveredItem === item.label.toLowerCase()
                      ? "text-zinc-100"
                      : "text-zinc-400 hover:text-zinc-100",
                  )}
                  onMouseEnter={() => {
                    setHoveredItem(item.label.toLowerCase());
                    setActiveDropdown(null);
                  }}
                >
                  <span className="relative z-10">{item.label}</span>
                </button>
              ) : (
                <button
                  type="button"
                  key={item.menu}
                  className="relative flex h-9 cursor-pointer items-center rounded-xl px-4 py-2 text-sm text-zinc-400 capitalize transition-colors hover:text-zinc-100"
                  onMouseEnter={() => handleMouseEnter(item.menu)}
                >
                  {hoveredItem === item.menu && (
                    <div className="absolute inset-0 h-full w-full rounded-xl bg-zinc-800 transition-all duration-300 ease-out" />
                  )}
                  <div className="relative z-10 flex items-center gap-2">
                    <span>
                      {item.label.charAt(0).toUpperCase() + item.label.slice(1)}
                    </span>
                    <HugeiconsIcon
                      icon={ArrowDown01Icon}
                      size={17}
                      className={cn(
                        "transition duration-200",
                        hoveredItem === item.menu && "rotate-180",
                      )}
                    />
                  </div>
                </button>
              ),
            )}
          </div>

          <div className="flex items-center gap-2">
            {cta || <RaisedButton color="#00bbff">Get Started</RaisedButton>}
          </div>
        </div>

        <AnimatePresence>
          {activeDropdown && (
            <NavbarMenu activeMenu={activeDropdown} sections={sections} />
          )}
        </AnimatePresence>
      </div>
    </div>
  );
}
```

---

### Nested Menu <a name="nested-menu"></a>

A hover-activated nested menu component for multi-level navigation with smooth animations.

- **Registry Key**: `nested-menu`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/nested-menu.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-popover`

#### How to Use

##### Example 1

```tsx
import { NestedMenu } from "@/components/ui/nested-menu";
import { Button } from "@/components/ui/button";
import { Settings, ChevronRight } from "lucide-react";

const menuSections = [
  {
    title: "Actions",
    items: [
      {
        key: "settings",
        label: "Settings",
        icon: Settings,
        onSelect: () => console.log("Settings clicked"),
      },
      {
        key: "download",
        label: "Download",
        hasSubmenu: true,
        submenuItems: [
          { key: "mac", label: "macOS", onSelect: () => {} },
          { key: "windows", label: "Windows", onSelect: () => {} },
        ],
      },
    ],
  },
];

export default function Example() {
  return (
    <NestedMenu
      sections={menuSections}
      trigger={<Button>Open Menu</Button>}
      arrowIcon={ChevronRight}
    />
  );
}
```

##### Example 2

```tsx
import { useNestedMenu, NestedMenuTooltip } from "@/components/ui/nested-menu";

function MyComponent() {
  const resourcesMenu = useNestedMenu();

  return (
    <>
      <button
        onMouseEnter={resourcesMenu.handleMouseEnter}
        onMouseLeave={resourcesMenu.handleMouseLeave}
      >
        Resources
      </button>

      <NestedMenuTooltip
        isOpen={resourcesMenu.isOpen}
        onOpenChange={resourcesMenu.setIsOpen}
        itemRef={resourcesMenu.itemRef}
        menuItems={[
          { key: "docs", label: "Documentation", onSelect: () => {} },
          { key: "blog", label: "Blog", onSelect: () => {} },
        ]}
        onMouseEnter={resourcesMenu.cancelClose}
        onMouseLeave={resourcesMenu.handleMouseLeave}
      />
    </>
  );
}
```

##### Example 3

```tsx
const [open, setOpen] = useState(false);

<NestedMenu
  sections={sections}
  trigger={<Button>Menu</Button>}
  open={open}
  onOpenChange={setOpen}
/>
```

##### Example 4

```tsx
const sections = [
  {
    items: [
      {
        key: "twitter",
        label: "Twitter",
        icon: TwitterIcon,
        iconColor: "#1da1f2",
        onSelect: () => {},
      },
      {
        key: "discord",
        label: "Discord",
        icon: DiscordIcon,
        iconColor: "#5865F2",
        onSelect: () => {},
      },
    ],
  },
];
```

##### Example 5

```tsx
const sections = [
  {
    items: [
      {
        key: "logout",
        label: "Sign Out",
        icon: LogoutIcon,
        variant: "danger",
        onSelect: () => {},
      },
    ],
  },
];
```

#### Props & API Reference

##### `NestedMenu` Props

| Prop          | Type                                     | Default   | Description                           |
| ------------- | ---------------------------------------- | --------- | ------------------------------------- |
| sections      | NestedMenuSectionProps[]                 | -         | Menu sections (required)              |
| trigger       | React.ReactNode                          | -         | Trigger element (required)            |
| iconClassName | string                                   | "h-4 w-4" | Icon className                        |
| arrowIcon     | React.ComponentType                      | undefined | Arrow icon for submenu items          |
| side          | "top" \| "right" \| "bottom" \| "left"   | "right"   | Menu placement                        |
| align         | "start" \| "center" \| "end"             | "start"   | Alignment relative to trigger         |
| sideOffset    | number                                   | 8         | Offset from trigger                   |
| className     | string                                   | undefined | Container className                   |
| open          | boolean                                  | undefined | Controlled open state                 |
| onOpenChange  | (open: boolean) => void                  | undefined | Callback when open state changes      |

#### Component Source Code

##### File: `registry/new-york/ui/nested-menu.tsx`

```tsx
"use client";

import * as React from "react";
import { useState, useCallback, useRef, useEffect } from "react";
import * as PopoverPrimitive from "@radix-ui/react-popover";
import { cn } from "@/lib/utils";

// ============================================================================
// Types
// ============================================================================

export interface NestedMenuItem {
	/** Unique key for the item */
	key: string;
	/** Display label */
	label: string;
	/** Icon component */
	icon?: React.ComponentType<{ className?: string }>;
	/** Click handler */
	onSelect?: () => void;
	/** Whether this item has a submenu */
	hasSubmenu?: boolean;
	/** Submenu items (if hasSubmenu is true) */
	submenuItems?: NestedMenuItem[];
	/** Color variant */
	variant?: "default" | "danger";
	/** Additional className for custom styling */
	className?: string;
	/** Icon color (CSS color value) */
	iconColor?: string;
	/** Separator after this item */
	separator?: boolean;
}

export interface NestedMenuSectionProps {
	/** Section title */
	title?: string;
	/** Items in this section */
	items: NestedMenuItem[];
	/** Show divider after section */
	showDivider?: boolean;
}

// ============================================================================
// Hook: useNestedMenu
// ============================================================================

export function useNestedMenu() {
	const [isOpen, setIsOpen] = useState(false);
	const [itemRef, setItemRef] = useState<HTMLElement | null>(null);
	const timeoutRef = useRef<NodeJS.Timeout | null>(null);

	const handleMouseEnter = useCallback((e: React.MouseEvent<HTMLElement>) => {
		if (timeoutRef.current) {
			clearTimeout(timeoutRef.current);
			timeoutRef.current = null;
		}
		setItemRef(e.currentTarget as HTMLElement);
		setIsOpen(true);
	}, []);

	const handleMouseLeave = useCallback(() => {
		// Longer delay to allow moving to submenu
		timeoutRef.current = setTimeout(() => {
			setIsOpen(false);
		}, 300);
	}, []);

	const cancelClose = useCallback(() => {
		if (timeoutRef.current) {
			clearTimeout(timeoutRef.current);
			timeoutRef.current = null;
		}
	}, []);

	useEffect(() => {
		return () => {
			if (timeoutRef.current) {
				clearTimeout(timeoutRef.current);
			}
		};
	}, []);

	return {
		isOpen,
		setIsOpen,
		itemRef,
		handleMouseEnter,
		handleMouseLeave,
		cancelClose,
	};
}

// ============================================================================
// NestedMenuTooltip Component
// ============================================================================

export interface NestedMenuTooltipProps {
	/** Whether the tooltip is open */
	isOpen: boolean;
	/** Callback when open state changes */
	onOpenChange: (open: boolean) => void;
	/** Reference element to anchor the tooltip */
	itemRef: HTMLElement | null;
	/** Menu items to display */
	menuItems: NestedMenuItem[];
	/** Icon className */
	iconClassName?: string;
	/** Container className */
	className?: string;
	/** Called when mouse enters the tooltip */
	onMouseEnter?: () => void;
	/** Called when mouse leaves the tooltip */
	onMouseLeave?: () => void;
}

export function NestedMenuTooltip({
	isOpen,
	onOpenChange,
	itemRef,
	menuItems,
	iconClassName = "h-4 w-4",
	className,
	onMouseEnter,
	onMouseLeave,
}: NestedMenuTooltipProps) {
	if (!itemRef || !isOpen) return null;

	const rect = itemRef.getBoundingClientRect();

	return (
		<PopoverPrimitive.Root open={isOpen} onOpenChange={onOpenChange}>
			<PopoverPrimitive.Anchor
				style={{
					position: "fixed",
					left: rect.right,
					top: rect.top,
					width: 1,
					height: 1,
					pointerEvents: "none",
				}}
			/>
			<PopoverPrimitive.Portal>
				<PopoverPrimitive.Content
					side="right"
					align="start"
					sideOffset={10}
					className={cn(
						"z-50 min-w-[180px] overflow-hidden rounded-lg bg-popover p-1 text-popover-foreground shadow-lg animate-in fade-in-0 zoom-in-95",
						"data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2",
						className,
					)}
					onMouseEnter={onMouseEnter}
					onMouseLeave={onMouseLeave}
					onOpenAutoFocus={(e) => e.preventDefault()}
					onCloseAutoFocus={(e) => e.preventDefault()}
				>
					{menuItems.map((item) => {
						const Icon = item.icon;
						return (
							<button
								type="button"
								key={item.key}
								className={cn(
									"flex w-full items-center gap-2 rounded-md px-3 py-2 text-sm outline-none transition-colors",
									"hover:bg-accent hover:text-accent-foreground focus:bg-accent focus:text-accent-foreground",
									item.variant === "danger" &&
										"text-destructive hover:bg-destructive/10 hover:text-destructive focus:bg-destructive/10",
									item.className,
								)}
								onClick={() => {
									item.onSelect?.();
									onOpenChange(false);
								}}
							>
								{Icon && (
									<span
										style={
											item.iconColor ? { color: item.iconColor } : undefined
										}
									>
										<Icon className={cn(iconClassName)} />
									</span>
								)}
								<span className="flex-1 text-left">{item.label}</span>
							</button>
						);
					})}
				</PopoverPrimitive.Content>
			</PopoverPrimitive.Portal>
		</PopoverPrimitive.Root>
	);
}

// ============================================================================
// NestedMenuItem Component
// ============================================================================

export interface NestedMenuItemComponentProps {
	/** Menu item data */
	item: NestedMenuItem;
	/** Icon className */
	iconClassName?: string;
	/** Whether to show arrow for submenu */
	showSubmenuArrow?: boolean;
	/** Arrow icon component */
	arrowIcon?: React.ComponentType<{ className?: string }>;
	/** Mouse enter handler (for submenu trigger) */
	onMouseEnter?: (e: React.MouseEvent<HTMLElement>) => void;
	/** Mouse leave handler */
	onMouseLeave?: () => void;
}

export function NestedMenuItemComponent({
	item,
	iconClassName = "h-6 w-6",
	showSubmenuArrow = true,
	arrowIcon: ArrowIcon,
	onMouseEnter,
	onMouseLeave,
}: NestedMenuItemComponentProps) {
	const Icon = item.icon;

	return (
		<button
			type="button"
			className={cn(
				"flex w-full items-center gap-2 rounded-lg px-3 py-2 text-sm outline-none transition-colors",
				"hover:bg-accent hover:text-accent-foreground focus:bg-accent focus:text-accent-foreground",
				item.variant === "danger" &&
					"text-destructive hover:bg-destructive/10 hover:text-destructive focus:bg-destructive/10",
				item.className,
			)}
			onClick={() => !item.hasSubmenu && item.onSelect?.()}
			onMouseEnter={onMouseEnter}
			onMouseLeave={onMouseLeave}
		>
			{Icon && (
				<span style={item.iconColor ? { color: item.iconColor } : undefined}>
					<Icon className={cn(iconClassName)} />
				</span>
			)}
			<span className="flex-1 text-left">{item.label}</span>
			{item.hasSubmenu && showSubmenuArrow && ArrowIcon && (
				<ArrowIcon className="h-4 w-4 text-muted-foreground" />
			)}
		</button>
	);
}

// ============================================================================
// NestedMenuSection Component
// ============================================================================

export interface NestedMenuSectionComponentProps
	extends NestedMenuSectionProps {
	/** Icon className */
	iconClassName?: string;
	/** Arrow icon for submenu items */
	arrowIcon?: React.ComponentType<{ className?: string }>;
	/** Callback when item with submenu is hovered */
	onSubmenuHover?: (
		item: NestedMenuItem,
		e: React.MouseEvent<HTMLElement>,
	) => void;
	/** Callback when mouse leaves submenu trigger */
	onSubmenuLeave?: () => void;
}

export function NestedMenuSection({
	title,
	items,
	showDivider = false,
	iconClassName = "h-4 w-4",
	arrowIcon,
	onSubmenuHover,
	onSubmenuLeave,
}: NestedMenuSectionComponentProps) {
	return (
		<div className="py-1">
			{title && (
				<div className="px-2 py-1.5 text-xs font-medium text-muted-foreground">
					{title}
				</div>
			)}
			{items.map((item) => (
				<React.Fragment key={item.key}>
					<NestedMenuItemComponent
						item={item}
						iconClassName={iconClassName}
						arrowIcon={arrowIcon}
						onMouseEnter={
							item.hasSubmenu ? (e) => onSubmenuHover?.(item, e) : undefined
						}
						onMouseLeave={item.hasSubmenu ? onSubmenuLeave : undefined}
					/>
					{item.separator && <div className="my-1 h-px bg-border" />}
				</React.Fragment>
			))}
			{showDivider && <div className="my-1 h-px bg-border" />}
		</div>
	);
}

// ============================================================================
// NestedMenu Component (Full Menu with built-in submenu handling)
// ============================================================================

export interface NestedMenuProps {
	/** Menu sections */
	sections: NestedMenuSectionProps[];
	/** Trigger element */
	trigger: React.ReactNode;
	/** Icon className */
	iconClassName?: string;
	/** Arrow icon for submenu items */
	arrowIcon?: React.ComponentType<{ className?: string }>;
	/** Menu placement */
	side?: "top" | "right" | "bottom" | "left";
	/** Alignment relative to trigger */
	align?: "start" | "center" | "end";
	/** Offset from trigger */
	sideOffset?: number;
	/** Container className */
	className?: string;
	/** Controlled open state */
	open?: boolean;
	/** Callback when open state changes */
	onOpenChange?: (open: boolean) => void;
}

export function NestedMenu({
	sections,
	trigger,
	iconClassName = "h-4 w-4",
	arrowIcon,
	side = "right",
	align = "start",
	sideOffset = 8,
	className,
	open: controlledOpen,
	onOpenChange,
}: NestedMenuProps) {
	const [internalOpen, setInternalOpen] = useState(false);
	const isControlled = controlledOpen !== undefined;
	const open = isControlled ? controlledOpen : internalOpen;

	const handleOpenChange = useCallback(
		(newOpen: boolean) => {
			if (!isControlled) {
				setInternalOpen(newOpen);
			}
			onOpenChange?.(newOpen);
		},
		[isControlled, onOpenChange],
	);

	// Submenu state
	const [activeSubmenu, setActiveSubmenu] = useState<NestedMenuItem | null>(
		null,
	);
	const [submenuAnchor, setSubmenuAnchor] = useState<HTMLElement | null>(null);
	const submenuTimeoutRef = useRef<NodeJS.Timeout | null>(null);

	const handleSubmenuHover = useCallback(
		(item: NestedMenuItem, e: React.MouseEvent<HTMLElement>) => {
			if (submenuTimeoutRef.current) {
				clearTimeout(submenuTimeoutRef.current);
				submenuTimeoutRef.current = null;
			}
			setActiveSubmenu(item);
			setSubmenuAnchor(e.currentTarget);
		},
		[],
	);

	const handleSubmenuLeave = useCallback(() => {
		submenuTimeoutRef.current = setTimeout(() => {
			setActiveSubmenu(null);
			setSubmenuAnchor(null);
		}, 300);
	}, []);

	const handleSubmenuEnter = useCallback(() => {
		if (submenuTimeoutRef.current) {
			clearTimeout(submenuTimeoutRef.current);
			submenuTimeoutRef.current = null;
		}
	}, []);

	useEffect(() => {
		return () => {
			if (submenuTimeoutRef.current) {
				clearTimeout(submenuTimeoutRef.current);
			}
		};
	}, []);

	// Close submenu when main menu closes
	useEffect(() => {
		if (!open) {
			setActiveSubmenu(null);
			setSubmenuAnchor(null);
		}
	}, [open]);

	return (
		<>
			<PopoverPrimitive.Root open={open} onOpenChange={handleOpenChange}>
				<PopoverPrimitive.Trigger asChild>{trigger}</PopoverPrimitive.Trigger>
				<PopoverPrimitive.Portal>
					<PopoverPrimitive.Content
						side={side}
						align={align}
						sideOffset={sideOffset}
						className={cn(
							"z-50 min-w-[200px] overflow-hidden rounded-xl px-2 py-1 bg-popover text-popover-foreground shadow-lg animate-in fade-in-0 zoom-in-95",
							"data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2",
							className,
						)}
						onOpenAutoFocus={(e) => e.preventDefault()}
						onCloseAutoFocus={(e) => e.preventDefault()}
						onInteractOutside={(e) => {
							// Prevent closing when interacting with submenu
							const target = e.target as HTMLElement;
							if (target?.closest("[data-radix-popper-content-wrapper]")) {
								e.preventDefault();
							}
						}}
					>
						{sections.map((section, index) => (
							<NestedMenuSection
								key={section.title || `section-${index}`}
								{...section}
								iconClassName={iconClassName}
								arrowIcon={arrowIcon}
								onSubmenuHover={handleSubmenuHover}
								onSubmenuLeave={handleSubmenuLeave}
							/>
						))}
					</PopoverPrimitive.Content>
				</PopoverPrimitive.Portal>
			</PopoverPrimitive.Root>

			{/* Submenu */}
			{activeSubmenu?.submenuItems && (
				<NestedMenuTooltip
					isOpen={!!activeSubmenu}
					onOpenChange={(isOpen) => {
						if (!isOpen) {
							setActiveSubmenu(null);
							setSubmenuAnchor(null);
						}
					}}
					itemRef={submenuAnchor}
					menuItems={activeSubmenu.submenuItems}
					iconClassName={iconClassName}
					onMouseEnter={handleSubmenuEnter}
					onMouseLeave={handleSubmenuLeave}
				/>
			)}
		</>
	);
}

export default NestedMenu;
```

---

### Notification Card <a name="notification-card"></a>

An enhanced notification card with action buttons, read/unread states, and timestamp display.

- **Registry Key**: `notification-card`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/notification-card.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
import { NotificationCard } from "@/components/ui/notification-card";

export default function Example() {
  return (
    <NotificationCard
      id="1"
      title="New message"
      body="You have a new message from the team."
      status="unread"
      createdAt={new Date()}
      actions={[
        { id: "view", label: "View", type: "redirect", style: "primary" },
        { id: "dismiss", label: "Dismiss", type: "api_call" },
      ]}
      onAction={(notifId, actionId) => {
        console.log(`Action ${actionId} on notification ${notifId}`);
      }}
    />
  );
}
```

#### Props & API Reference

##### `NotificationCard` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| id | string | - | Unique notification identifier |
| title | string | - | Notification title |
| body | string | - | Notification message |
| status | "unread" \| "read" \| "archived" | "unread" | Read status |
| createdAt | string \| Date | - | Creation timestamp |
| actions | NotificationAction[] | [] | Available actions |
| onMarkAsRead | (id) => void | - | Mark as read handler |
| onAction | (notifId, actionId) => void | - | Action button handler |
| loadingActionId | string | - | Action currently loading |

#### Component Source Code

##### File: `registry/new-york/ui/notification-card.tsx`

```tsx

```

---

### Pie Chart <a name="pie-chart"></a>

A pie chart for proportions with donut, label, and legend display modes.

- **Registry Key**: `pie-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/pie-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `chart`
  - `card`

#### How to Use

```tsx
import { PieChart } from "@/components/ui/pie-chart";

const data = [
  { source: "Direct", visitors: 1240 },
  { source: "Search", visitors: 980 },
  { source: "Social", visitors: 620 },
];

export default function Example() {
  return (
    <PieChart
      data={data}
      nameKey="source"
      valueKey="visitors"
      title="Traffic Sources"
    />
  );
}
```

#### Props & API Reference

##### `PieChart` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| data | Array&lt;Record&lt;string, string \| number&gt;&gt; | - | Chart data rows |
| nameKey | string | - | Key for slice name |
| valueKey | string | - | Key for numeric slice value |
| title | string | - | Card title |
| description | string | - | Card description |
| footer | string | - | Optional footer text |
| mode | "donut" \| "legend" \| "label" | "donut" | Display mode |

#### Component Source Code

##### File: `registry/new-york/ui/pie-chart.tsx`

```tsx
"use client";

import * as React from "react";
import {
	Cell,
	Label,
	Pie,
	PieChart as RechartsPieChart,
} from "recharts";
import { Card, CardContent, CardFooter, CardHeader } from "@/components/ui/card";
import {
	type ChartConfig,
	ChartContainer,
	ChartLegend,
	ChartLegendContent,
	ChartTooltip,
	ChartTooltipContent,
} from "@/components/ui/chart";

const PIE_COLORS = ["#00bbff", "#34d399", "#60a5fa", "#a78bfa", "#f472b6"];

function ChartCard({
	title,
	description,
	footer,
	children,
	dataPoints = 0,
}: {
	title?: string;
	description?: string;
	footer?: string;
	children: React.ReactNode;
	dataPoints?: number;
}) {
	const width =
		dataPoints > 0 ? Math.min(768, Math.max(300, dataPoints * 80)) : undefined;
	return (
		<Card
			className="max-w-3xl"
			style={width ? { width } : { width: "100%" }}
		>
			{(title || description) && (
				<CardHeader className="pb-0">
					{title && (
						<p className="text-sm font-semibold text-card-foreground">
							{title}
						</p>
					)}
					{description && (
						<p className="text-xs text-muted-foreground">{description}</p>
					)}
				</CardHeader>
			)}
			<CardContent>{children}</CardContent>
			{footer && (
				<CardFooter>
					<p className="text-xs text-muted-foreground">{footer}</p>
				</CardFooter>
			)}
		</Card>
	);
}

export type PieChartProps = {
	data: Array<Record<string, string | number>>;
	nameKey: string;
	valueKey: string;
	title?: string;
	description?: string;
	footer?: string;
	mode?: "donut" | "legend" | "label";
};

export function PieChart(props: PieChartProps) {
	const mode = props.mode ?? "donut";
	const chartConfig: ChartConfig = Object.fromEntries(
		props.data.map((entry, i) => {
			const name = String(entry[props.nameKey] ?? i);
			return [name, { label: name, color: PIE_COLORS[i % PIE_COLORS.length] }];
		}),
	);
	const total = React.useMemo(
		() =>
			props.data.reduce((sum, entry) => {
				const val = entry[props.valueKey];
				return sum + (typeof val === "number" ? val : 0);
			}, 0),
		[props.data, props.valueKey],
	);
	return (
		<ChartCard
			title={props.title}
			description={props.description}
			footer={props.footer}
			dataPoints={props.data.length}
		>
			<ChartContainer
				config={chartConfig}
				className={`h-50 w-full ${mode === "label" ? "[&_.recharts-pie-label-text]:fill-foreground" : ""}`}
			>
				<RechartsPieChart>
					{mode === "donut" ? (
						<Pie
							data={props.data}
							dataKey={props.valueKey}
							nameKey={props.nameKey}
							cx="50%"
							cy="50%"
							outerRadius={80}
							innerRadius={60}
							strokeWidth={0}
						>
							{props.data.map((entry, i) => (
								<Cell
									key={String(entry[props.nameKey])}
									fill={PIE_COLORS[i % PIE_COLORS.length]}
								/>
							))}
							<Label
								content={({ viewBox }) => {
									if (viewBox && "cx" in viewBox && "cy" in viewBox) {
										return (
											<text
												x={viewBox.cx}
												y={viewBox.cy}
												textAnchor="middle"
												dominantBaseline="middle"
											>
												<tspan
													x={viewBox.cx}
													y={viewBox.cy}
													className="fill-foreground text-2xl font-bold"
												>
													{total}
												</tspan>
												<tspan
													x={viewBox.cx}
													y={(viewBox.cy || 0) + 20}
													fontSize={12}
													fill="#71717a"
												>
													{props.valueKey}
												</tspan>
											</text>
										);
									}
								}}
							/>
						</Pie>
					) : mode === "label" ? (
						<Pie
							data={props.data}
							dataKey={props.valueKey}
							nameKey={props.nameKey}
							cx="50%"
							cy="50%"
							outerRadius={70}
							strokeWidth={0}
							label
						>
							{props.data.map((entry, i) => (
								<Cell
									key={String(entry[props.nameKey])}
									fill={PIE_COLORS[i % PIE_COLORS.length]}
								/>
							))}
						</Pie>
					) : (
						<Pie
							data={props.data}
							dataKey={props.valueKey}
							nameKey={props.nameKey}
							cx="50%"
							cy="50%"
							outerRadius={70}
							strokeWidth={0}
						>
							{props.data.map((entry, i) => (
								<Cell
									key={String(entry[props.nameKey])}
									fill={PIE_COLORS[i % PIE_COLORS.length]}
								/>
							))}
						</Pie>
					)}
					<ChartTooltip
						content={<ChartTooltipContent nameKey={props.nameKey} />}
					/>
					{mode === "legend" && (
						<ChartLegend
							content={<ChartLegendContent nameKey={props.nameKey} />}
							className="-translate-y-2 flex-wrap gap-2"
						/>
					)}
				</RechartsPieChart>
			</ChartContainer>
		</ChartCard>
	);
}
```

---

### Pricing Card <a name="pricing-card"></a>

A beautiful glassmorphism pricing card component with nested card design, feature lists, and customizable accent colors.

- **Registry Key**: `pricing-card`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/pricing-card.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `icons`

#### How to Use

##### Example 1

```tsx
import { PricingCard } from "@/components/ui/pricing-card";

export default function Example() {
  return (
    <PricingCard
      title="Pro"
      price={9.99}
      description="Everything you need to be productive."
      features={[
        "Unlimited conversations",
        "Priority support",
        "Custom integrations",
      ]}
      onButtonClick={() => console.log("Selected!")}
    />
  );
}
```

##### Example 2

```tsx
<PricingCard
  title="Free"
  price={0}
  description="Get started for free."
  features={["5 conversations per day", "Basic AI", "Community support"]}
  buttonLabel="Get Started"
/>
```

##### Example 3

```tsx
<PricingCard
  title="Pro"
  price={9}
  originalPrice={12}
  isMonthly={false}
  description="Best value for power users."
  features={["Unlimited everything", "Priority support", "All integrations"]}
  accentColor="#00bbff"
/>
```

##### Example 4

```tsx
<PricingCard
  title="Pro"
  price={12}
  isCurrentPlan={true}
  features={["All Pro features"]}
/>
```

##### Example 5

```tsx
const features = [
  "Simple string feature",
  {
    text: "Feature with custom icon",
    icon: <CustomIcon className="h-4 w-4" />,
  },
];
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `title` | `string` | - | Plan title (e.g., "Free", "Pro") |
| `price` | `number` | - | Price amount |
| `currency` | `string` | `"$"` | Currency symbol |
| `originalPrice` | `number` | - | Original price (shows strikethrough) |
| `description` | `string` | - | Plan description/subtitle |
| `features` | `(string \| PricingFeature)[]` | - | List of included features |
| `featuresTitle` | `ReactNode` | - | Custom title above features |
| `isMonthly` | `boolean` | `true` | Billing period display |
| `isCurrentPlan` | `boolean` | `false` | Shows "Current Plan" badge |
| `buttonLabel` | `string` | Auto | Custom button label |
| `onButtonClick` | `() => void` | - | Button click handler |
| `isDisabled` | `boolean` | `false` | Disable the button |
| `isLoading` | `boolean` | `false` | Show loading state |
| `accentColor` | `string` | `"#3b82f6"` | Button accent color |
| `className` | `string` | - | Additional CSS classes |

#### Component Source Code

##### File: `registry/new-york/ui/pricing-card.tsx`

```tsx
"use client";

import type { FC, ReactNode } from "react";
import { CheckmarkCircle02Icon, HugeiconsIcon } from "@/components/icons";
import { cn } from "@/lib/utils";
import { RaisedButton } from "@/registry/new-york/ui/raised-button";

export interface PricingFeature {
	text: string;
	icon?: ReactNode;
}

export interface PricingCardProps {
	/** Plan title (e.g., "Free", "Pro", "Enterprise") */
	title: string;
	/** Price amount (e.g., 0, 9.99, 29) */
	price: number;
	/** Currency symbol (default: "$") */
	currency?: string;
	/** Original price before discount (shows strikethrough on yearly) */
	originalPrice?: number;
	/** Plan description/subtitle — always reserves two lines for row alignment */
	description?: string;
	/** Features included in this plan */
	features?: (string | PricingFeature)[];
	/** Custom label above features list (rendered uppercase, muted) */
	featuresTitle?: ReactNode;
	/** Billing period: true for monthly, false for yearly */
	isMonthly?: boolean;
	/** Show "Current Plan" badge */
	isCurrentPlan?: boolean;
	/** Show "Popular" badge in header */
	isPopular?: boolean;
	/** Marks this card as the highlighted Pro tier (tints feature icons) */
	isPro?: boolean;
	/** Marks this card as the Enterprise tier (changes accent + suffix copy) */
	isEnterprise?: boolean;
	/** Optional plan illustration shown above the price */
	planImage?: string;
	/** Override the price display (e.g., "Custom") */
	priceLabel?: string;
	/** Override the line beneath the price (defaults to "/ per month" etc.) */
	priceSuffix?: ReactNode;
	/** Button label (default depends on plan + state) */
	buttonLabel?: string;
	/** Small text rendered under the CTA button */
	buttonFootnote?: ReactNode;
	/** Button click handler */
	onButtonClick?: () => void;
	/** Whether button is disabled */
	isDisabled?: boolean;
	/** Whether button is in loading state */
	isLoading?: boolean;
	/** Accent color for button (CSS color value) */
	accentColor?: string;
	/** Additional CSS classes */
	className?: string;
}

const formatPrice = (amount: number, currency: string): string => {
	if (amount === 0) return `${currency}0`;
	return `${currency}${amount.toFixed(amount % 1 === 0 ? 0 : 2)}`;
};

export const PricingCard: FC<PricingCardProps> = ({
	title,
	price,
	currency = "$",
	originalPrice,
	description,
	features,
	featuresTitle,
	isMonthly = true,
	isCurrentPlan = false,
	isPopular = false,
	isPro = false,
	isEnterprise = false,
	planImage,
	priceLabel,
	priceSuffix,
	buttonLabel,
	buttonFootnote,
	onButtonClick,
	isDisabled = false,
	isLoading = false,
	accentColor,
	className,
}) => {
	const isFree = price === 0 && !isEnterprise;

	const formattedPrice = formatPrice(price, currency);
	const formattedOriginalPrice = originalPrice
		? formatPrice(originalPrice, currency)
		: null;

	// Yearly Pro: show per-month equivalent above an "/ mo, billed $X / year" line
	const monthlyEquivalent =
		!isMonthly && isPro && price > 0
			? formatPrice(Math.round(price / 12), currency)
			: null;

	const resolvedAccent =
		accentColor ?? (isEnterprise ? "#fa4b00" : isFree ? "#2a2a2a" : "#00bbff");

	const getButtonLabel = (): string => {
		if (isLoading) return "Loading...";
		if (buttonLabel) return buttonLabel;
		if (isCurrentPlan) return "Current Plan";
		if (isFree) return "Start for Free";
		if (isEnterprise) return "Contact Sales";
		return "Get Started";
	};

	const resolvedSuffix: ReactNode = (() => {
		if (priceSuffix !== undefined) return priceSuffix;
		if (isEnterprise) return "Tailored pricing";
		if (price <= 0) return " ";
		if (monthlyEquivalent) return `/ mo, billed ${formattedPrice} / year`;
		return isMonthly ? "/ per month" : "/ per year";
	})();

	const resolvedFootnote: ReactNode = (() => {
		if (buttonFootnote !== undefined) return buttonFootnote;
		if (isEnterprise) return "Reply within 24 hours";
		if (isFree) return "No credit card required";
		return "Cancel anytime · Secure payment";
	})();

	return (
		<div
			className={cn(
				"flex h-full w-full flex-col overflow-hidden rounded-3xl",
				"bg-white/70 dark:bg-zinc-800/50 backdrop-blur-lg",
				className,
			)}
		>
			{/* Header: plan name + badges (reserves a fixed row height) */}
			<div className="flex flex-col gap-1.5 p-6 pb-4">
				<div className="flex min-h-5 items-center justify-between">
					<div className="flex items-center gap-2">
						<span className="text-2xl font-semibold text-zinc-900 dark:text-white">
							{title}
						</span>
						{isCurrentPlan && (
							<span className="rounded-full bg-green-500/15 px-2 py-0.5 text-xs font-medium text-green-700 dark:text-green-400">
								Current Plan
							</span>
						)}
					</div>
					{isPopular && !isCurrentPlan && (
						<span className="rounded-full bg-primary/15 px-2 py-0.5 text-xs font-medium tracking-wide text-primary">
							Popular
						</span>
					)}
				</div>

				{/* Description — always reserve two lines so cards align */}
				<p className="line-clamp-2 min-h-10 text-sm font-light leading-relaxed text-zinc-600 dark:text-zinc-400">
					{description ?? " "}
				</p>
			</div>

			{planImage && (
				<div className="px-6 pb-5">
					<div className="relative h-40 w-full overflow-hidden rounded-2xl">
						{/* eslint-disable-next-line @next/next/no-img-element */}
						<img
							src={planImage}
							alt={`${title} plan`}
							className="h-full w-full object-cover transition duration-300 hover:scale-125"
						/>
					</div>
				</div>
			)}

			{/* Price */}
			<div className="px-6 pb-5">
				<div className="flex items-baseline gap-2">
					{formattedOriginalPrice && !isMonthly && !priceLabel && (
						<span className="text-2xl font-normal text-zinc-500 line-through">
							{formattedOriginalPrice}
						</span>
					)}
					<span className="text-5xl font-semibold tracking-tight text-zinc-900 dark:text-white">
						{priceLabel ?? monthlyEquivalent ?? formattedPrice}
					</span>
				</div>
				<p className="mt-1 text-sm font-normal text-zinc-500 dark:text-zinc-400">
					{resolvedSuffix}
				</p>
			</div>

			{/* CTA */}
			<div className="px-6 pb-5">
				<RaisedButton
					onClick={onButtonClick}
					disabled={isDisabled || isLoading || isCurrentPlan}
					className="w-full"
					color={resolvedAccent}
				>
					{getButtonLabel()}
				</RaisedButton>
				{resolvedFootnote && (
					<p className="mt-2 text-center text-xs font-light text-zinc-500">
						{resolvedFootnote}
					</p>
				)}
			</div>

			{/* Features — flex-1 so cards in a row equalize their height */}
			{(featuresTitle || (features && features.length > 0)) && (
				<div className="flex flex-1 flex-col gap-2.5 px-6 py-5">
					{featuresTitle && (
						<p className="mb-0.5 text-xs font-normal uppercase text-zinc-500">
							{featuresTitle}
						</p>
					)}

					{features?.map((feature) => {
						const featureText =
							typeof feature === "string" ? feature : feature.text;
						const featureIcon =
							typeof feature === "object" && feature.icon ? (
								feature.icon
							) : (
								<HugeiconsIcon
									icon={CheckmarkCircle02Icon}
									size={15}
									className={cn(
										"mt-0.5 shrink-0",
										isPro || isEnterprise
											? "text-primary"
											: "text-zinc-500",
									)}
								/>
							);

						return (
							<div
								key={featureText}
								className="flex items-start gap-3 text-sm font-light text-zinc-700 dark:text-zinc-300"
							>
								<span className="flex-shrink-0">{featureIcon}</span>
								<span>{featureText}</span>
							</div>
						);
					})}
				</div>
			)}
		</div>
	);
};

export default PricingCard;
```

---

### Radar Chart <a name="radar-chart"></a>

A radar chart for multi-axis comparisons across several dimensions.

- **Registry Key**: `radar-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/radar-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `chart`
  - `card`

#### How to Use

```tsx
import { RadarChart } from "@/components/ui/radar-chart";

const data = [
  { skill: "Speed", you: 82, team: 70 },
  { skill: "Accuracy", you: 91, team: 75 },
  { skill: "Reliability", you: 76, team: 80 },
];

export default function Example() {
  return (
    <RadarChart
      data={data}
      angleKey="skill"
      valueKeys={["you", "team"]}
      title="Performance"
    />
  );
}
```

#### Props & API Reference

##### `RadarChart` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| data | Array&lt;Record&lt;string, string \| number&gt;&gt; | - | Chart data rows |
| angleKey | string | - | Key on each row used for the axis label |
| valueKeys | string \| string[] | - | One or more numeric keys to plot |
| title | string | - | Card title |
| description | string | - | Card description |
| footer | string | - | Optional footer text |
| colors | string[] | CHART_COLORS | Custom palette for series |

#### Component Source Code

##### File: `registry/new-york/ui/radar-chart.tsx`

```tsx
"use client";

import type * as React from "react";
import {
	PolarAngleAxis,
	PolarGrid,
	Radar,
	RadarChart as RechartsRadarChart,
} from "recharts";
import { Card, CardContent, CardFooter, CardHeader } from "@/components/ui/card";
import {
	type ChartConfig,
	ChartContainer,
	ChartLegend,
	ChartLegendContent,
	ChartTooltip,
	ChartTooltipContent,
} from "@/components/ui/chart";

const CHART_COLORS = ["#00bbff", "#34d399", "#60a5fa", "#f472b6", "#fb923c"];

function toKeys(v: string | string[]): string[] {
	return Array.isArray(v) ? v : [v];
}

function ChartCard({
	title,
	description,
	footer,
	children,
	dataPoints = 0,
}: {
	title?: string;
	description?: string;
	footer?: string;
	children: React.ReactNode;
	dataPoints?: number;
}) {
	const width =
		dataPoints > 0 ? Math.min(768, Math.max(300, dataPoints * 80)) : undefined;
	return (
		<Card
			className="max-w-3xl"
			style={width ? { width } : { width: "100%" }}
		>
			{(title || description) && (
				<CardHeader className="pb-0">
					{title && (
						<p className="text-sm font-semibold text-card-foreground">
							{title}
						</p>
					)}
					{description && (
						<p className="text-xs text-muted-foreground">{description}</p>
					)}
				</CardHeader>
			)}
			<CardContent>{children}</CardContent>
			{footer && (
				<CardFooter>
					<p className="text-xs text-muted-foreground">{footer}</p>
				</CardFooter>
			)}
		</Card>
	);
}

export type RadarChartProps = {
	data: Array<Record<string, string | number>>;
	angleKey: string;
	valueKeys: string | string[];
	title?: string;
	description?: string;
	footer?: string;
	colors?: string[];
};

export function RadarChart(props: RadarChartProps) {
	const keys = toKeys(props.valueKeys);
	const colors = props.colors ?? CHART_COLORS;
	const chartConfig: ChartConfig = Object.fromEntries(
		keys.map((key, i) => [
			key,
			{ label: key, color: colors[i % colors.length] },
		]),
	);
	const showLegend = keys.length > 1;
	return (
		<ChartCard
			title={props.title}
			description={props.description}
			footer={props.footer}
			dataPoints={props.data.length}
		>
			<ChartContainer config={chartConfig} className="h-55 w-full">
				<RechartsRadarChart data={props.data}>
					<PolarGrid stroke="#3f3f46" />
					<PolarAngleAxis
						dataKey={props.angleKey}
						tick={({ x, y, textAnchor, index, ...tickProps }) => {
							const d = props.data[index] as Record<string, string | number>;
							const yNum = typeof y === "number" ? y : 0;
							const vals = keys.map((k) => d[k]).join(" / ");
							return (
								<text
									x={x}
									y={yNum + (index === 0 ? -10 : 0)}
									textAnchor={textAnchor}
									fontSize={12}
									{...tickProps}
								>
									<tspan fill="#e4e4e7" fontWeight={500}>
										{vals}
									</tspan>
									<tspan x={x} dy="1.1em" fontSize={11} fill="#71717a">
										{String(d[props.angleKey])}
									</tspan>
								</text>
							);
						}}
					/>
					<ChartTooltip content={<ChartTooltipContent />} />
					{keys.map((key) => (
						<Radar
							key={key}
							dataKey={key}
							stroke={`var(--color-${key})`}
							strokeWidth={2}
							fill={`var(--color-${key})`}
							fillOpacity={0.2}
						/>
					))}
					{showLegend && <ChartLegend content={<ChartLegendContent />} />}
				</RechartsRadarChart>
			</ChartContainer>
		</ChartCard>
	);
}
```

---

### Raised Button <a name="raised-button"></a>

A beautiful 3D raised button with customizable colors and dynamic text contrast.

- **Registry Key**: `raised-button`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/raised-button.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`

#### How to Use

```tsx
import { RaisedButton } from "@/components/ui/raised-button";

export default function Example() {
  return <RaisedButton>Click me</RaisedButton>;
}
```

#### Props & API Reference

##### `Props` Props

| Prop      | Type                                | Default   | Description                           |
| --------- | ----------------------------------- | --------- | ------------------------------------- |
| size      | "sm" \| "default" \| "lg" \| "icon" | "default" | Size of the button                    |
| color     | string                              | undefined | Custom color (hex, rgb, or CSS color) |
| disabled  | boolean                             | false     | Whether the button is disabled        |
| className | string                              | undefined | Additional CSS classes                |

#### Component Source Code

##### File: `registry/new-york/ui/raised-button.tsx`

```tsx
"use client";

import { cva, type VariantProps } from "class-variance-authority";
import * as React from "react";

import { cn } from "@/lib/utils";
import {
	getContrastColor,
	getLuminance,
	parseColor,
} from "@/lib/utils/colorUtils";

const buttonVariants = cva(
	"inline-flex items-center justify-center dark:bg-zinc-500 dark:text-white whitespace-nowrap text-sm font-medium transition-all focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 relative bg-primary text-primary-foreground hover:bg-primary/90 border border-primary/50 shadow-md before:absolute before:inset-0 before:border-t before:border-white/40 before:bg-gradient-to-b before:from-white/20 before:to-transparent cursor-pointer transition-transform duration-200 active:scale-[0.96] subpixel-antialiased gap-2",
	{
		variants: {
			variant: {
				default: "",
			},
			size: {
				default: "h-10 px-4 py-2 rounded-xl before:rounded-xl",
				sm: "h-9 rounded-lg px-3 before:rounded-xl",
				lg: "h-11 rounded-lg px-8 before:rounded-lg",
				icon: "h-10 w-10",
			},
		},
		defaultVariants: {
			variant: "default",
			size: "default",
		},
	},
);

export interface ButtonProps
	extends React.ButtonHTMLAttributes<HTMLButtonElement>,
		VariantProps<typeof buttonVariants> {
	color?: string;
}

const RaisedButton = React.forwardRef<HTMLButtonElement, ButtonProps>(
	({ className, variant, size, color, style = {}, ...props }, ref) => {
		const Comp = "button";

		const dynamicStyles = React.useMemo(() => {
			if (!color) return {};

			try {
				const rgb = parseColor(color);
				if (!rgb) return {};

				const luminance = getLuminance(rgb);
				const textColor = getContrastColor(luminance);
				const borderOpacity = 0.5;
				const hoverOpacity = 0.9;
				const whiteBorderOpacity = 0.6;
				const whiteGradientOpacity = 0.3;
				const shadowOpacity = 0.2;
				const shadowSpread = "0px";
				const shadowBlur = "5px";

				return {
					backgroundColor: color,
					color: textColor,
					borderColor: `rgba(${rgb.r}, ${rgb.g}, ${rgb.b}, ${borderOpacity})`,
					"--hover-bg": `rgba(${rgb.r}, ${rgb.g}, ${rgb.b}, ${hoverOpacity})`,
					"--border": `rgba(255, 255, 255, ${whiteBorderOpacity})`,
					"--gradient": `rgba(255, 255, 255, ${whiteGradientOpacity})`,
					"--shadow-color": `rgba(${rgb.r}, ${rgb.g}, ${rgb.b}, ${shadowOpacity})`,
					boxShadow: `0 4px ${shadowBlur} ${shadowSpread} var(--shadow-color)`,
					transition: "all 0.2s ease-in-out",
				};
			} catch (e) {
				console.error("Error processing color:", e);
				return {};
			}
		}, [color]);

		const computedClassName = cn(
			buttonVariants({ variant, size, className }),
			color &&
				"hover:bg-[color:var(--hover-bg)] before:border-[color:var(--border)] before:from-[color:var(--gradient)] hover:opacity-80 overflow-hidden",
		);

		return (
			<Comp
				className={computedClassName}
				ref={ref}
				style={{
					...style,
					...dynamicStyles,
				}}
				{...props}
			/>
		);
	},
);
RaisedButton.displayName = "RaisedButton";

export { buttonVariants, RaisedButton };
```

##### File: `lib/utils/colorUtils.ts`

```tsx
const CSS_COLOR_NAMES: Record<string, [number, number, number]> = {
	black: [0, 0, 0],
	white: [255, 255, 255],
	red: [255, 0, 0],
	green: [0, 128, 0],
	blue: [0, 0, 255],
	yellow: [255, 255, 0],
	cyan: [0, 255, 255],
	magenta: [255, 0, 255],
	gray: [128, 128, 128],
	grey: [128, 128, 128],
	orange: [255, 165, 0],
	purple: [128, 0, 128],
	pink: [255, 192, 203],
	brown: [165, 42, 42],
	navy: [0, 0, 128],
	teal: [0, 128, 128],
	olive: [128, 128, 0],
	maroon: [128, 0, 0],
	silver: [192, 192, 192],
	lime: [0, 255, 0],
	aqua: [0, 255, 255],
	fuchsia: [255, 0, 255],
};

export function hexToRgb(
	hex: string,
): { r: number; g: number; b: number } | null {
	const normalizedHex = hex.charAt(0) === "#" ? hex.substring(1) : hex;

	if (normalizedHex.length === 3) {
		const r = parseInt(normalizedHex.charAt(0) + normalizedHex.charAt(0), 16);
		const g = parseInt(normalizedHex.charAt(1) + normalizedHex.charAt(1), 16);
		const b = parseInt(normalizedHex.charAt(2) + normalizedHex.charAt(2), 16);
		return { r, g, b };
	}

	if (normalizedHex.length === 6) {
		const r = parseInt(normalizedHex.substring(0, 2), 16);
		const g = parseInt(normalizedHex.substring(2, 4), 16);
		const b = parseInt(normalizedHex.substring(4, 6), 16);
		return { r, g, b };
	}

	if (normalizedHex !== hex) {
		try {
			const tempElement = document.createElement("div");
			tempElement.style.color = hex;
			document.body.appendChild(tempElement);

			const computedColor = window.getComputedStyle(tempElement).color;
			document.body.removeChild(tempElement);

			const rgbMatch = computedColor.match(
				/rgba?\((\d+),\s*(\d+),\s*(\d+)(?:,\s*[\d.]+)?\)/,
			);
			if (rgbMatch) {
				return {
					r: parseInt(rgbMatch[1], 10),
					g: parseInt(rgbMatch[2], 10),
					b: parseInt(rgbMatch[3], 10),
				};
			}
		} catch (e) {
			console.error("Error processing color name:", e);
		}
	}

	return null;
}

export function getLuminance(rgb: { r: number; g: number; b: number }): number {
	const { r, g, b } = rgb;

	const sR = r / 255;
	const sG = g / 255;
	const sB = b / 255;

	const R = sR <= 0.03928 ? sR / 12.92 : ((sR + 0.055) / 1.055) ** 2.4;
	const G = sG <= 0.03928 ? sG / 12.92 : ((sG + 0.055) / 1.055) ** 2.4;
	const B = sB <= 0.03928 ? sB / 12.92 : ((sB + 0.055) / 1.055) ** 2.4;

	return 0.2126 * R + 0.7152 * G + 0.0722 * B;
}

export function getContrastColor(luminance: number): string {
	return luminance > 0.5 ? "#000000" : "#ffffff";
}

export function parseColor(
	color: string,
): { r: number; g: number; b: number } | null {
	const hexResult = hexToRgb(color);
	if (hexResult) return hexResult;

	try {
		if (typeof document !== "undefined") {
			const tempElement = document.createElement("div");
			tempElement.style.color = color;
			document.body.appendChild(tempElement);

			const computedColor = window.getComputedStyle(tempElement).color;
			document.body.removeChild(tempElement);

			const rgbMatch = computedColor.match(
				/rgba?\((\d+),\s*(\d+),\s*(\d+)(?:,\s*[\d.]+)?\)/,
			);
			if (rgbMatch) {
				return {
					r: parseInt(rgbMatch[1], 10),
					g: parseInt(rgbMatch[2], 10),
					b: parseInt(rgbMatch[3], 10),
				};
			}
		} else {
			const colorMap: Record<string, { r: number; g: number; b: number }> = {};
			for (const [name, rgb] of Object.entries(CSS_COLOR_NAMES)) {
				const [r, g, b] = rgb;
				colorMap[name.toLowerCase()] = { r, g, b };
			}

			const normalizedColorName = color.toLowerCase().trim();
			if (colorMap[normalizedColorName]) {
				return colorMap[normalizedColorName];
			}
		}

		const rgbDirectMatch = color.match(
			/rgba?\((\d+),\s*(\d+),\s*(\d+)(?:,\s*[\d.]+)?\)/,
		);
		if (rgbDirectMatch) {
			return {
				r: parseInt(rgbDirectMatch[1], 10),
				g: parseInt(rgbDirectMatch[2], 10),
				b: parseInt(rgbDirectMatch[3], 10),
			};
		}

		const hslMatch = color.match(
			/hsla?\((\d+),\s*(\d+)%,\s*(\d+)%(?:,\s*[\d.]+)?\)/,
		);
		if (hslMatch) {
			const h = parseInt(hslMatch[1], 10) / 360;
			const s = parseInt(hslMatch[2], 10) / 100;
			const l = parseInt(hslMatch[3], 10) / 100;

			return hslToRgb(h, s, l);
		}
	} catch (e) {
		console.error("Error processing color:", e);
	}

	return null;
}

export function hslToRgb(
	h: number,
	s: number,
	l: number,
): { r: number; g: number; b: number } {
	let r: number, g: number, b: number;

	if (s === 0) {
		r = g = b = l * 255;
	} else {
		const hue2rgb = (p: number, q: number, t: number): number => {
			if (t < 0) t += 1;
			if (t > 1) t -= 1;
			if (t < 1 / 6) return p + (q - p) * 6 * t;
			if (t < 1 / 2) return q;
			if (t < 2 / 3) return p + (q - p) * (2 / 3 - t) * 6;
			return p;
		};

		const q = l < 0.5 ? l * (1 + s) : l + s - l * s;
		const p = 2 * l - q;

		r = hue2rgb(p, q, h + 1 / 3) * 255;
		g = hue2rgb(p, q, h) * 255;
		b = hue2rgb(p, q, h - 1 / 3) * 255;
	}

	return {
		r: Math.round(r),
		g: Math.round(g),
		b: Math.round(b),
	};
}
```

---

### Scatter Chart <a name="scatter-chart"></a>

A scatter chart for exploring correlation between two numeric variables.

- **Registry Key**: `scatter-chart`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/scatter-chart.json
```

#### Dependencies

- **NPM Packages**:
  - `recharts`
- **Local Registry Dependencies**:
  - `chart`
  - `card`

#### How to Use

```tsx
import { ScatterChart } from "@/components/ui/scatter-chart";

const data = [
  { hours: 1, productivity: 22 },
  { hours: 2, productivity: 35 },
  { hours: 3, productivity: 48 },
];

export default function Example() {
  return (
    <ScatterChart
      data={data}
      xKey="hours"
      yKey="productivity"
      title="Hours vs. Productivity"
    />
  );
}
```

#### Props & API Reference

##### `ScatterChart` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| data | Array&lt;Record&lt;string, string \| number&gt;&gt; | - | Chart data rows |
| xKey | string | - | Key for the X axis |
| yKey | string | - | Key for the Y axis |
| title | string | - | Card title |
| description | string | - | Card description |
| footer | string | - | Optional footer text |
| labelKey | string | - | Optional key used for point labels |

#### Component Source Code

##### File: `registry/new-york/ui/scatter-chart.tsx`

```tsx
"use client";

import type * as React from "react";
import {
	ScatterChart as RechartsScatterChart,
	Scatter,
	XAxis,
	YAxis,
} from "recharts";
import { Card, CardContent, CardFooter, CardHeader } from "@/components/ui/card";
import {
	type ChartConfig,
	ChartContainer,
	ChartTooltip,
	ChartTooltipContent,
} from "@/components/ui/chart";

const CHART_COLORS = ["#00bbff", "#34d399", "#60a5fa", "#f472b6", "#fb923c"];

function ChartCard({
	title,
	description,
	footer,
	children,
	dataPoints = 0,
}: {
	title?: string;
	description?: string;
	footer?: string;
	children: React.ReactNode;
	dataPoints?: number;
}) {
	const width =
		dataPoints > 0 ? Math.min(768, Math.max(300, dataPoints * 80)) : undefined;
	return (
		<Card
			className="max-w-3xl"
			style={width ? { width } : { width: "100%" }}
		>
			{(title || description) && (
				<CardHeader className="pb-0">
					{title && (
						<p className="text-sm font-semibold text-card-foreground">
							{title}
						</p>
					)}
					{description && (
						<p className="text-xs text-muted-foreground">{description}</p>
					)}
				</CardHeader>
			)}
			<CardContent>{children}</CardContent>
			{footer && (
				<CardFooter>
					<p className="text-xs text-muted-foreground">{footer}</p>
				</CardFooter>
			)}
		</Card>
	);
}

export type ScatterChartProps = {
	data: Array<Record<string, string | number>>;
	xKey: string;
	yKey: string;
	title?: string;
	description?: string;
	footer?: string;
	labelKey?: string;
};

export function ScatterChart(props: ScatterChartProps) {
	const chartConfig: ChartConfig = {
		scatter: { label: "Data", color: CHART_COLORS[0] },
	};
	return (
		<ChartCard
			title={props.title}
			description={props.description}
			footer={props.footer}
			dataPoints={props.data.length}
		>
			<ChartContainer config={chartConfig} className="h-50 w-full">
				<RechartsScatterChart>
					<XAxis
						dataKey={props.xKey}
						axisLine={false}
						tickLine={false}
						tick={{ fill: "#71717a", fontSize: 11 }}
					/>
					<YAxis
						dataKey={props.yKey}
						axisLine={false}
						tickLine={false}
						tick={{ fill: "#71717a", fontSize: 11 }}
					/>
					<ChartTooltip content={<ChartTooltipContent />} />
					<Scatter data={props.data} fill="var(--color-scatter)" />
				</RechartsScatterChart>
			</ChartContainer>
		</ChartCard>
	);
}
```

---

### Search Results Tabs <a name="search-results-tabs"></a>

A beautiful component for displaying search results with web pages, images, and news in an organized, interactive layout.

- **Registry Key**: `search-results-tabs`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/search-results-tabs.json
```

#### Dependencies

- **NPM Packages**:
  - `framer-motion`
- **Local Registry Dependencies**:
  - `icons`

#### How to Use

##### Example 1

```tsx
import { SearchResultsTabs } from "@/components/ui/search-results-tabs";

export default function Example() {
  const searchResults = {
    web: [
      {
        title: "Example Site",
        url: "https://example.com",
        content: "Description of the website...",
      },
    ],
    images: [
      "https://images.unsplash.com/photo-1...",
      "https://images.unsplash.com/photo-2...",
    ],
    news: [
      {
        title: "News Article",
        url: "https://news.example.com",
        content: "Article content...",
        score: 0.95,
        date: "2025-11-10",
      },
    ],
  };

  return <SearchResultsTabs search_results={searchResults} />;
}
```

##### Example 2

```tsx
interface SearchResults {
  web?: WebResult[];
  images?: string[] | ImageResult[];
  news?: NewsResult[];
}
```

##### Example 3

```tsx
interface WebResult {
  title: string;
  url: string;
  content: string;
  date?: string;
}
```

##### Example 4

```tsx
interface ImageResult {
  url: string;
  title?: string;
}
```

##### Example 5

```tsx
interface NewsResult {
  title: string;
  url: string;
  content: string;
  score?: number;
  date?: string;
}
```

##### Example 6

```tsx
<SearchResultsTabs
  search_results={{
    web: [
      {
        title: "Example",
        url: "https://example.com",
        content: "Description",
      },
    ],
  }}
/>
```

##### Example 7

```tsx
<SearchResultsTabs
  search_results={{
    images: [
      "https://example.com/image1.jpg",
      "https://example.com/image2.jpg",
    ],
  }}
/>
```

##### Example 8

```tsx
const [selectedImage, setSelectedImage] = useState<string | null>(null);

<SearchResultsTabs
  search_results={{ images: [...] }}
  onImageClick={setSelectedImage}
/>
```

##### Example 9

```tsx
<SearchResultsTabs
  search_results={{
    news: [
      {
        title: "Article Title",
        url: "https://news.example.com",
        content: "Article excerpt...",
        score: 0.95,
        date: "2025-11-10",
      },
    ],
  }}
/>
```

#### Props & API Reference

##### `Props` Props

| Prop           | Type                       | Default   | Description                             |
| -------------- | -------------------------- | --------- | --------------------------------------- |
| search_results | SearchResults              | -         | Object containing web, images, and news |
| onImageClick   | (imageUrl: string) => void | undefined | Callback when an image is clicked       |

#### Component Source Code

##### File: `registry/new-york/ui/search-results-tabs.tsx`

```tsx
"use client";

import { motion } from "framer-motion";
import Image from "next/image";
import { useCallback, useEffect, useState } from "react";
import { HugeiconsIcon, News01Icon } from "@/components/icons";

import { Button } from "@/components/ui/button";
import {
	Popover,
	PopoverContent,
	PopoverTrigger,
} from "@/components/ui/popover";
import { Skeleton } from "@/components/ui/skeleton";

export interface ImageResult {
	url: string;
	title?: string;
}

export interface WebResult {
	title: string;
	url: string;
	content: string;
	date?: string;
}

export interface NewsResult {
	title: string;
	url: string;
	content: string;
	score?: number;
	date?: string;
}

export interface SearchResults {
	web?: WebResult[];
	images?: string[] | ImageResult[];
	news?: NewsResult[];
}

interface SearchResultsTabsProps {
	search_results: SearchResults;
	onImageClick?: (imageUrl: string) => void;
}

export function SearchResultsTabs({
	search_results,
	onImageClick,
}: SearchResultsTabsProps) {
	return (
		<div className="w-full">
			<div className="space-y-6">
				{search_results.web && search_results.web?.length > 0 && (
					<SourcesButton web={search_results.web} />
				)}

				{search_results.images && search_results.images?.length > 0 && (
					<ImageResults
						images={search_results.images}
						onImageClick={onImageClick}
					/>
				)}

				{search_results.news && search_results.news?.length > 0 && (
					<NewsResults news={search_results.news} />
				)}
			</div>
		</div>
	);
}

interface ImageResultsProps {
	images: string[] | ImageResult[];
	onImageClick?: (imageUrl: string) => void;
}

function ImageResults({ images, onImageClick }: ImageResultsProps) {
	const [validImages, setValidImages] = useState<string[]>([]);

	useEffect(() => {
		const validateImages = async () => {
			// Convert to string array if needed
			const imageUrls = images.map((img) =>
				typeof img === "string" ? img : img.url,
			);

			// Filter out obviously invalid images first
			const potentiallyValidImages = imageUrls.filter(
				(imageUrl) => imageUrl && typeof imageUrl === "string",
			);

			// Test each image by trying to load it
			const validationPromises = potentiallyValidImages.map(
				(imageUrl) =>
					new Promise<string | null>((resolve) => {
						const img = new window.Image();

						const timeoutId = setTimeout(() => {
							resolve(null); // Timeout after 5 seconds
						}, 5000);

						img.onload = () => {
							clearTimeout(timeoutId);
							resolve(imageUrl);
						};

						img.onerror = () => {
							clearTimeout(timeoutId);
							resolve(null);
						};

						img.src = imageUrl;
					}),
			);

			try {
				const results = await Promise.all(validationPromises);
				const validImageUrls = results.filter(
					(url): url is string => url !== null,
				);
				setValidImages(validImageUrls);
			} catch (error) {
				console.error("Error validating images:", error);
				setValidImages([]);
			}
		};

		if (images && images.length > 0) validateImages();
		else setValidImages([]);
	}, [images]);

	if (validImages.length === 0) {
		return null;
	}

	return (
		<div className="my-4 flex w-full max-w-2xl -space-x-15 pr-2">
			{validImages.map((imageUrl, index) => (
				<ImageItem
					key={imageUrl}
					imageUrl={imageUrl}
					index={index}
					onImageClick={() => onImageClick?.(imageUrl)}
					totalImages={validImages.length}
				/>
			))}
		</div>
	);
}

interface ImageItemProps {
	imageUrl: string;
	index: number;
	onImageClick: () => void;
	totalImages: number;
}

function ImageItem({
	imageUrl,
	index,
	onImageClick,
	totalImages,
}: ImageItemProps) {
	const [isLoading, setIsLoading] = useState(true);

	const handleLoad = useCallback(() => {
		setIsLoading(false);
	}, []);

	return (
		<motion.div
			onClick={onImageClick}
			className={`group cursor-pointer overflow-hidden rounded-2xl shadow-lg transition-all duration-200 ${
				(index + 1) % 2 === 0
					? "-rotate-7 hover:-rotate-0"
					: "rotate-7 hover:rotate-0"
			}`}
			style={{
				zIndex: index,
			}}
			initial={{ scale: 0.6, filter: "blur(10px)" }}
			animate={{ scale: 1, filter: "blur(0px)" }}
			transition={{
				delay: index * 0.1,
				duration: 0.1,
				ease: [0.19, 1, 0.22, 1],
				scale: {
					duration: 0.2,
				},
			}}
			onMouseEnter={(e) => {
				e.currentTarget.style.zIndex = (totalImages + 10).toString();
			}}
			onMouseLeave={(e) => {
				e.currentTarget.style.zIndex = index.toString();
			}}
		>
			{isLoading && (
				<div className="absolute inset-0 z-10">
					<Skeleton className="aspect-square h-full w-full rounded-2xl" />
				</div>
			)}
			<Image
				src={imageUrl}
				alt={`Search result image ${index + 1}`}
				width={700}
				height={700}
				className={`aspect-square h-full bg-zinc-800 object-cover transition ${
					isLoading ? "opacity-0" : "opacity-100"
				}`}
				onLoad={handleLoad}
				priority={index < 3}
			/>
		</motion.div>
	);
}

interface SourcesButtonProps {
	web: WebResult[];
}

function SourcesButton({ web }: SourcesButtonProps) {
	return (
		<div className="flex justify-start">
			<Popover>
				<PopoverTrigger asChild>
					<Button variant="secondary" size="sm" className="rounded-full">
						<div className="flex -space-x-3">
							{web.slice(0, 4).map((result, index) => (
								<div
									key={index}
									className="flex h-5 w-5 items-center justify-center rounded-full border-2 border-background bg-muted"
								>
									<Image
										src={`https://www.google.com/s2/favicons?domain=${
											new URL(result.url).hostname
										}&sz=64`}
										alt={`${new URL(result.url).hostname} favicon`}
										width={16}
										height={16}
										className="h-full w-full rounded-full"
										onError={(e) => {
											const target = e.target as HTMLImageElement;
											target.style.display = "none";
										}}
									/>
								</div>
							))}
						</div>
						<span className="font-medium">Search Results</span>
					</Button>
				</PopoverTrigger>
				<PopoverContent className="w-full max-w-lg bg-background/95 p-0 backdrop-blur-xl">
					<WebResults web={web} />
				</PopoverContent>
			</Popover>
		</div>
	);
}

interface NewsResultsProps {
	news: NewsResult[];
}

function NewsResults({ news }: NewsResultsProps) {
	return (
		<div className="space-y-2">
			{news.map((article, index) => (
				<div
					key={index}
					className="overflow-hidden rounded-lg border bg-card p-4 shadow-sm transition-all hover:shadow-md"
				>
					<div className="flex flex-row items-center gap-2 transition-all hover:text-primary">
						<HugeiconsIcon icon={News01Icon} size={20} />
						<h2 className="truncate text-lg font-medium">
							<a href={article.url} target="_blank" rel="noopener noreferrer">
								{article.title}
							</a>
						</h2>
					</div>
					<p className="mb-1 line-clamp-2 text-sm text-muted-foreground">
						{article.content}
					</p>
					<div className="flex flex-wrap items-center gap-x-4 text-sm text-muted-foreground">
						{article.score && (
							<span className="text-xs">Score: {article.score.toFixed(2)}</span>
						)}
						{article.date && (
							<span className="text-xs">
								{new Date(article.date).toLocaleDateString()}
							</span>
						)}
					</div>
				</div>
			))}
		</div>
	);
}

interface WebResultsProps {
	web: WebResult[];
}

function WebResults({ web }: WebResultsProps) {
	return (
		<div className="max-h-80 w-full overflow-y-auto">
			{web.map((result, index) => (
				<div
					className="w-full border-b p-4 pb-3 transition-all hover:bg-accent/50"
					key={index}
				>
					<a
						href={result.url}
						target="_blank"
						rel="noopener noreferrer"
						className="w-full space-y-1"
					>
						<h2 className="truncate text-sm font-medium">{result.title}</h2>
						<p className="line-clamp-2 text-xs text-muted-foreground">
							{result.content}
						</p>
						<div className="flex flex-wrap items-center gap-x-4 text-sm">
							<span className="flex items-center gap-2">
								<Image
									src={`https://www.google.com/s2/favicons?domain=${
										new URL(result.url).hostname
									}&sz=64`}
									alt={`${new URL(result.url).hostname} favicon`}
									width={16}
									height={16}
									className="rounded-full"
									onError={(e) => {
										const target = e.target as HTMLImageElement;
										target.style.display = "none";
									}}
								/>
								<span className="max-w-xs truncate text-xs text-primary hover:underline">
									{new URL(result.url).hostname}
								</span>
							</span>
							{result.date && (
								<span className="text-xs text-muted-foreground">
									{new Date(result.date).toLocaleDateString()}
								</span>
							)}
						</div>
					</a>
				</div>
			))}
		</div>
	);
}
```

---

### Slash Command Dropdown <a name="slash-command-dropdown"></a>

A slash command dropdown for selecting tools and actions with categorization and search.

- **Registry Key**: `slash-command-dropdown`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/slash-command-dropdown.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `icons`

#### How to Use

```tsx
import { SlashCommandDropdown } from "@/components/ui/slash-command-dropdown";

const tools = [
  { name: "send_email", category: "email", description: "Send an email" },
  { name: "create_event", category: "calendar", description: "Create event" },
  { name: "create_todo", category: "todos", description: "Create a todo" },
];

const matches = tools.map((tool) => ({
  tool,
  score: 1.0,
}));

export default function Example() {
  return (
    <SlashCommandDropdown
      matches={matches}
      selectedIndex={0}
      onSelect={(match) => console.log("Selected:", match.tool.name)}
      onClose={() => console.log("Closed")}
      position={{ top: 100, left: 50 }}
      isVisible={true}
    />
  );
}
```

#### Props & API Reference

##### `SlashCommandDropdown` Props

| Prop             | Type                              | Default | Description                              |
| ---------------- | --------------------------------- | ------- | ---------------------------------------- |
| matches          | SlashCommandMatch[]               | -       | Array of matched tools with scores       |
| selectedIndex    | number                            | -       | Currently selected item index            |
| onSelect         | (tool: SlashCommandMatch) => void | -       | Callback when a tool is selected         |
| onClose          | () => void                        | -       | Callback when dropdown should close      |
| position         | object                            | -       | Position of the dropdown                 |
| isVisible        | boolean                           | -       | Whether the dropdown is visible          |
| openedViaButton  | boolean                           | false   | Whether opened via button (affects focus)|
| selectedCategory | string                            | "all"   | Currently selected category filter       |
| categories       | string[]                          | []      | Available categories for filtering       |
| onCategoryChange | (category: string) => void        | -       | Callback when category filter changes    |

#### Component Source Code

##### File: `registry/new-york/ui/slash-command-dropdown.tsx`

```tsx
"use client";

import type React from "react";
import { useEffect, useMemo, useRef } from "react";
import {
	Cancel01Icon,
	HugeiconsIcon,
	Tag01Icon,
	ToolsIcon,
} from "@/components/icons";

import { cn } from "@/lib/utils";
import { formatToolName, getToolCategoryIcon } from "@/lib/utils/tool-icons";

export interface Tool {
	/** Unique tool identifier */
	name: string;
	/** Category for grouping tools */
	category: string;
	/** Description shown below tool name */
	description?: string;
	/** Custom icon (defaults to category icon) */
	icon?: React.ReactNode;
}

export interface SlashCommandMatch {
	tool: Tool;
	score: number;
}

interface SlashCommandDropdownProps {
	/** List of tools to display */
	matches: SlashCommandMatch[];
	/** Currently selected tool index */
	selectedIndex: number;
	/** Callback when a tool is selected */
	onSelect: (tool: SlashCommandMatch) => void;
	/** Callback when dropdown is closed */
	onClose: () => void;
	/** Position config for the dropdown */
	position: { top?: number; bottom?: number; left: number; width?: number };
	/** Whether the dropdown is visible */
	isVisible: boolean;
	/** If opened via button (shows header) */
	openedViaButton?: boolean;
	/** Currently selected category filter */
	selectedCategory?: string;
	/** Available categories */
	categories?: string[];
	/** Callback when category changes */
	onCategoryChange?: (category: string) => void;
	/** Additional CSS classes */
	className?: string;
	/** Additional inline styles */
	style?: React.CSSProperties;
}

// Default icon size for consistency
const ICON_SIZE = 20;
const CATEGORY_ICON_SIZE = 16;

/**
 * Get a default icon for a tool based on its category
 * Always returns an icon - no tool should be without one
 */
const getDefaultToolIcon = (
	tool: Tool,
	size: number = ICON_SIZE,
): React.ReactNode => {
	// If tool has custom icon, use it
	if (tool.icon) return tool.icon;

	// Try to get category icon
	const categoryIcon = getToolCategoryIcon(tool.category, {
		showBackground: false,
		width: size,
		height: size,
	});

	// If category icon exists, use it
	if (categoryIcon) return categoryIcon;

	return (
		<HugeiconsIcon icon={ToolsIcon} size={size} className="text-zinc-400" />
	);
};

/**
 * Get icon for a category tab
 */
const getCategoryTabIcon = (category: string): React.ReactNode => {
	if (category === "all") {
		return (
			<HugeiconsIcon
				icon={Tag01Icon}
				size={CATEGORY_ICON_SIZE}
				className="text-current"
			/>
		);
	}

	const icon = getToolCategoryIcon(category, {
		showBackground: false,
		width: CATEGORY_ICON_SIZE,
		height: CATEGORY_ICON_SIZE,
	});

	// Fallback for categories without icons
	return (
		icon || (
			<HugeiconsIcon
				icon={ToolsIcon}
				size={CATEGORY_ICON_SIZE}
				className="text-current"
			/>
		)
	);
};

export const SlashCommandDropdown: React.FC<SlashCommandDropdownProps> = ({
	matches,
	selectedIndex,
	onSelect,
	onClose,
	position,
	isVisible,
	openedViaButton = false,
	selectedCategory = "all",
	categories = [],
	onCategoryChange,
	className,
	style,
}) => {
	const dropdownRef = useRef<HTMLDivElement>(null);
	const scrollContainerRef = useRef<HTMLDivElement>(null);

	// Focus the dropdown when it becomes visible (only when opened via button)
	useEffect(() => {
		if (isVisible && openedViaButton && dropdownRef.current) {
			requestAnimationFrame(() => {
				dropdownRef.current?.focus();
			});
		}
	}, [isVisible, openedViaButton]);

	// Get unique categories from matches if not provided
	const computedCategories = useMemo(() => {
		if (categories && categories.length > 0) {
			return categories;
		}
		const uniqueCategories = Array.from(
			new Set(matches.map((match) => match.tool.category)),
		);
		return ["all", ...uniqueCategories.sort()];
	}, [matches, categories]);

	// Filter matches based on selected category
	const filteredMatches = useMemo(() => {
		if (selectedCategory === "all") {
			return matches;
		}
		return matches.filter((match) => match.tool.category === selectedCategory);
	}, [matches, selectedCategory]);

	// Scroll to selected item when selectedIndex changes
	useEffect(() => {
		if (selectedIndex >= 0 && selectedIndex < filteredMatches.length) {
			const selectedElement = scrollContainerRef.current?.querySelector(
				`[data-index="${selectedIndex}"]`,
			);
			if (selectedElement) {
				selectedElement.scrollIntoView({
					behavior: "smooth",
					block: "nearest",
				});
			}
		}
	}, [selectedIndex, filteredMatches.length]);

	if (!isVisible || matches.length === 0) return null;

	return (
		<div
			ref={dropdownRef}
			className={cn(
				// Base styles
				"fixed z-[200] overflow-hidden rounded-2xl",
				// Light mode support
				"border border-zinc-200 bg-white/95 dark:border-zinc-800 dark:bg-zinc-900/95",
				"backdrop-blur-xl shadow-2xl",
				// Animation
				"animate-in fade-in-0 slide-in-from-bottom-2 duration-200",
				className,
			)}
			style={{
				...(position.top !== undefined && { top: 0, height: position.top }),
				...(position.bottom !== undefined && {
					bottom: `calc(100vh - ${position.bottom - 8}px)`,
					maxHeight: Math.min(position.bottom - 16, 400),
				}),
				left: position.left,
				width: position.width,
				...style,
			}}
			tabIndex={-1}
		>
			{/* Header section - Only show when opened via button */}
			{openedViaButton && (
				<div className="flex items-center justify-between pl-5 pr-2 py-1">
					<div className="text-xs font-semibold text-zinc-900 dark:text-zinc-100">
						Browse Tools
					</div>
					<button
						type="button"
						onClick={onClose}
						className="cursor-pointer rounded-full p-1.5 hover:bg-zinc-100 dark:hover:bg-zinc-800 text-zinc-500 hover:text-zinc-700 dark:text-zinc-400 dark:hover:text-zinc-200 transition-colors"
						aria-label="Close"
					>
						<HugeiconsIcon icon={Cancel01Icon} size={16} />
					</button>
				</div>
			)}

			{/* Category Tabs */}
			{computedCategories.length > 1 && (
				<div>
					<div className="flex overflow-x-auto px-3 py-1 gap-1.5 [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
						{computedCategories.map((category) => (
							<button
								type="button"
								key={category}
								onClick={() => onCategoryChange?.(category)}
								className={cn(
									// Base styles
									"flex items-center gap-2 rounded-lg px-3 py-2 text-xs font-medium whitespace-nowrap cursor-pointer transition-all",
									// Selected state
									selectedCategory === category
										? "bg-zinc-100 text-zinc-900 dark:bg-zinc-700/50 dark:text-white"
										: "text-zinc-500 hover:bg-zinc-50 hover:text-zinc-700 dark:text-zinc-400 dark:hover:bg-zinc-800/50 dark:hover:text-zinc-300",
								)}
							>
								{getCategoryTabIcon(category)}
								<span>
									{category === "all" ? "All" : formatToolName(category)}
								</span>
							</button>
						))}
					</div>
				</div>
			)}

			{/* Tool List */}
			<div
				ref={scrollContainerRef}
				className="max-h-[200px] overflow-y-auto py-1.5"
			>
				{filteredMatches.length === 0 ? (
					<div className="px-4 py-8 text-center text-sm text-zinc-500 dark:text-zinc-400">
						No tools found
					</div>
				) : (
					filteredMatches.map((match, index) => {
						const isSelected = index === selectedIndex;
						return (
							<button
								type="button"
								key={`${match.tool.category}-${match.tool.name}`}
								data-index={index}
								className={cn(
									// Base styles - full width with consistent padding
									"w-full text-left mx-0 px-3 py-0 cursor-pointer transition-all duration-150",
									// Selected/hover states with light mode support
									isSelected
										? "bg-zinc-100 dark:bg-zinc-700/40"
										: "hover:bg-zinc-50 dark:hover:bg-zinc-800/40",
								)}
								onClick={() => onSelect(match)}
							>
								<div className="flex items-center gap-3 py-2.5 px-1">
									{/* Icon container - fixed size for consistency */}
									<div className="flex-shrink-0 flex items-center justify-center w-8 h-8 rounded-lg bg-zinc-100 dark:bg-zinc-800">
										{getDefaultToolIcon(match.tool, ICON_SIZE)}
									</div>

									{/* Content - fills remaining space */}
									<div className="min-w-0 flex-1">
										<div className="flex items-center justify-between gap-3">
											<span className="truncate text-sm font-medium text-zinc-900 dark:text-zinc-100">
												{formatToolName(match.tool.name)}
											</span>
											{selectedCategory === "all" && (
												<span className="flex-shrink-0 rounded-md bg-zinc-100 dark:bg-zinc-800 px-2 py-0.5 text-xs text-zinc-500 dark:text-zinc-400">
													{formatToolName(match.tool.category)}
												</span>
											)}
										</div>
										{match.tool.description && (
											<p className="text-xs text-zinc-500 dark:text-zinc-400 mt-0.5 truncate">
												{match.tool.description}
											</p>
										)}
									</div>
								</div>
							</button>
						);
					})
				)}
			</div>
		</div>
	);
};
```

##### File: `lib/utils/tool-icons.tsx`

```tsx
import type { IconSvgElement } from "@hugeicons/react";
import Image from "next/image";
import {
	Brain02Icon,
	CheckListIcon,
	AlarmClockIcon,
	ComputerProgramming01Icon,
	File02Icon,
	HugeiconsIcon,
	Image02Icon,
	InformationCircleIcon,
	Link01Icon,
	Notification03Icon,
	PackageOpenIcon,
	SourceCodeCircleIcon,
	SquareArrowUpRight02Icon,
	Target02Icon,
	ToolsIcon,
} from "@/components/icons";

export interface IconProps {
	size?: number;
	width?: number;
	height?: number;
	strokeWidth?: number;
	className?: string;
	color?: string;
}

// Category-specific icons with colors
export interface IconConfig {
	icon: IconSvgElement | string;
	bgColor: string;
	bgColorLight?: string; // Light mode background
	iconColor: string;
	isImage?: boolean;
}

/**
 * Normalize a category/integration name for icon lookup
 */
const normalizeCategoryName = (name: string): string => {
	if (!name) return "general";
	return name
		.toLowerCase()
		.trim()
		.replace(/[\s-]+/g, "_")
		.replace(/_+/g, "_")
		.replace(/^_|_$/g, "");
};

// Alias mapping for backwards compatibility
const iconAliases: Record<string, string> = {
	calendar: "google_calendar",
};

// Tool category icon configs - matches gaia repo pattern
const iconConfigs: Record<string, IconConfig> = {
	// Integration icons (use images)
	gmail: {
		icon: "/images/icons/gmail.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	google_calendar: {
		icon: "/images/icons/googlecalendar.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	github: {
		icon: "/images/icons/github.png",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	linear: {
		icon: "/images/icons/linear.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	slack: {
		icon: "/images/icons/slack.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	google_docs: {
		icon: "/images/icons/google_docs.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	googlesheets: {
		icon: "/images/icons/googlesheets.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	search: {
		icon: "/images/icons/google.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	weather: {
		icon: "/images/icons/weather.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	notion: {
		icon: "/images/icons/notion.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	twitter: {
		icon: "/images/icons/twitter.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	linkedin: {
		icon: "/images/icons/linkedin.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	reddit: {
		icon: "/images/icons/reddit.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	figma: {
		icon: "/images/icons/figma.svg",
		bgColor: "bg-zinc-800",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-white",
		isImage: true,
	},
	
	// Category icons (use HugeIcons components)
	todos: {
		icon: CheckListIcon,
		bgColor: "bg-emerald-500/20 backdrop-blur",
		bgColorLight: "bg-emerald-500/20",
		iconColor: "text-emerald-400",
	},
	reminders: {
		icon: AlarmClockIcon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-blue-400",
	},
	documents: {
		icon: File02Icon,
		bgColor: "bg-orange-500/20 backdrop-blur",
		bgColorLight: "bg-orange-500/20",
		iconColor: "text-orange-400",
	},
	development: {
		icon: SourceCodeCircleIcon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-cyan-400",
	},
	memory: {
		icon: Brain02Icon,
		bgColor: "bg-indigo-500/20 backdrop-blur",
		bgColorLight: "bg-indigo-500/20",
		iconColor: "text-indigo-400",
	},
	creative: {
		icon: Image02Icon,
		bgColor: "bg-pink-500/20 backdrop-blur",
		bgColorLight: "bg-pink-500/20",
		iconColor: "text-pink-400",
	},
	goal_tracking: {
		icon: Target02Icon,
		bgColor: "bg-emerald-500/20 backdrop-blur",
		bgColorLight: "bg-emerald-500/20",
		iconColor: "text-emerald-400",
	},
	notifications: {
		icon: Notification03Icon,
		bgColor: "bg-yellow-500/20 backdrop-blur",
		bgColorLight: "bg-yellow-500/20",
		iconColor: "text-yellow-400",
	},
	support: {
		icon: InformationCircleIcon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-blue-400",
	},
	general: {
		icon: InformationCircleIcon,
		bgColor: "bg-gray-500/20 backdrop-blur",
		bgColorLight: "bg-gray-500/20",
		iconColor: "text-gray-400",
	},
	integrations: {
		icon: Link01Icon,
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
	},
	
	// Agent tool call categories
	handoff: {
		icon: SquareArrowUpRight02Icon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-sky-400",
	},
	retrieve_tools: {
		icon: PackageOpenIcon,
		bgColor: "bg-indigo-500/20 backdrop-blur",
		bgColorLight: "bg-indigo-500/20",
		iconColor: "text-indigo-400",
	},
	executor: {
		icon: ComputerProgramming01Icon,
		bgColor: "bg-teal-500/20 backdrop-blur",
		bgColorLight: "bg-teal-500/20",
		iconColor: "text-teal-400",
	},
	unknown: {
		icon: ToolsIcon,
		bgColor: "bg-zinc-500/20 backdrop-blur",
		bgColorLight: "bg-zinc-500/20",
		iconColor: "text-zinc-400",
	},
};

/**
 * Get icon for a tool category with optional URL-based icon fallback.
 * Supports built-in categories and custom integration icons via iconUrl.
 */
export const getToolCategoryIcon = (
	category: string,
	iconProps: Partial<IconProps> & { showBackground?: boolean } = {},
	iconUrl?: string | null,
) => {
	const { showBackground = true, ...restProps } = iconProps;

	const defaultProps = {
		size: restProps.size || 16,
		width: restProps.width || 20,
		height: restProps.height || 20,
		strokeWidth: restProps.strokeWidth || 2,
		className: restProps.className,
	};

	// Normalize
	const normalizedCategory = normalizeCategoryName(category);

	// Resolve aliases
	const aliasedCategory =
		iconAliases[normalizedCategory] ||
		iconAliases[category] ||
		normalizedCategory;

	const finalCategory = normalizeCategoryName(aliasedCategory);

	let config = iconConfigs[finalCategory];

	// Fallback search
	if (!config) {
		const normalizedConfigs = Object.entries(iconConfigs);
		const matchingConfig = normalizedConfigs.find(
			([key]) => normalizeCategoryName(key) === finalCategory,
		);
		if (matchingConfig) {
			config = matchingConfig[1];
		}
	}

	// If no predefined config found, try iconUrl fallback for custom integrations
	if (!config) {
		if (iconUrl) {
			const iconElement = (
				<Image
					alt={`${category} Icon`}
					width={defaultProps.width}
					height={defaultProps.height}
					className={`${restProps.className || ""} aspect-square object-contain`}
					src={iconUrl}
				/>
			);
			return showBackground ? (
				<div className="rounded-lg p-1 bg-zinc-700 dark:bg-zinc-700">{iconElement}</div>
			) : (
				iconElement
			);
		}
		return null;
	}

	// Render image or component icon
	const iconElement = config.isImage ? (
		<Image
			alt={`${category} Icon`}
			width={defaultProps.width}
			height={defaultProps.height}
			className={`${restProps.className || ""} aspect-square object-contain`}
			src={config.icon as string}
		/>
	) : (
		<HugeiconsIcon
			icon={config.icon as IconSvgElement}
			size={defaultProps.size}
			className={restProps.className || config.iconColor}
		/>
	);

	// Return with or without background based on showBackground prop
	// Using dark: prefix for proper light/dark mode support
	return showBackground ? (
		<div className={`rounded-lg p-1 ${config.bgColorLight || config.bgColor} dark:${config.bgColor}`}>
			{iconElement}
		</div>
	) : (
		iconElement
	);
};

// Format tool names from snake_case to Title Case
export const formatToolName = (name: string): string => {
	return name
		.split("_")
		.map((word) => word.charAt(0).toUpperCase() + word.slice(1).toLowerCase())
		.join(" ");
};
```

---

### Stat Row <a name="stat-row"></a>

A compact KPI tile with optional trend indicator for dashboards and summary panels.

- **Registry Key**: `stat-row`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/stat-row.json
```

#### Dependencies

- **NPM Packages**:
  - `@hugeicons/react`
  - `@hugeicons/core-free-icons`

#### How to Use

```tsx
import { StatRow } from "@/components/ui/stat-row";

export default function Example() {
  return (
    <StatRow
      title="Active Users"
      value="12,480"
      trend="up"
      trendLabel="+12.4%"
    />
  );
}
```

#### Props & API Reference

##### `StatRow` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| title | string | - | Label for the KPI |
| value | string \| number | - | The primary value to display |
| unit | string | - | Optional unit suffix (e.g. "%", "K") |
| trend | "up" \| "down" \| "neutral" | - | Direction of the trend |
| trendLabel | string | - | Text shown next to the trend icon |

#### Component Source Code

##### File: `registry/new-york/ui/stat-row.tsx`

```tsx
"use client";

import {
	ArrowDown01Icon,
	ArrowRight01Icon,
	ArrowUp01Icon,
} from "@hugeicons/core-free-icons";
import { HugeiconsIcon } from "@hugeicons/react";

export type StatRowProps = {
	title: string;
	value: string | number;
	unit?: string;
	trend?: "up" | "down" | "neutral";
	trendLabel?: string;
};

const TREND_STYLES: Record<string, { color: string }> = {
	up: { color: "text-emerald-500 dark:text-emerald-400" },
	down: { color: "text-red-500 dark:text-red-400" },
	neutral: { color: "text-muted-foreground" },
};

function TrendIcon({
	trend,
	className,
}: {
	trend: string;
	className?: string;
}) {
	if (trend === "up")
		return <HugeiconsIcon icon={ArrowUp01Icon} className={className} />;
	if (trend === "down")
		return <HugeiconsIcon icon={ArrowDown01Icon} className={className} />;
	return <HugeiconsIcon icon={ArrowRight01Icon} className={className} />;
}

export function StatRow(props: StatRowProps) {
	const trendStyle = props.trend ? TREND_STYLES[props.trend] : null;
	return (
		<div className="rounded-2xl bg-card text-card-foreground border p-5 w-fit min-w-50 h-full flex flex-col justify-between gap-2">
			<p className="text-xs text-muted-foreground truncate">{props.title}</p>
			<div className="flex items-end gap-1.5">
				<span className="text-4xl font-bold text-foreground leading-none">
					{props.value}
				</span>
				{props.unit && (
					<span className="text-sm text-muted-foreground mb-0.5">
						{props.unit}
					</span>
				)}
			</div>
			<div className="h-4 flex items-center">
				{trendStyle && props.trendLabel && props.trend ? (
					<div className={`flex items-center gap-1 ${trendStyle.color}`}>
						<TrendIcon
							trend={props.trend}
							className={`w-3.5 h-3.5 ${trendStyle.color}`}
						/>
						<span className="text-xs font-medium">{props.trendLabel}</span>
					</div>
				) : null}
			</div>
		</div>
	);
}
```

---

### Todo Item <a name="todo-item"></a>

An interactive todo item component with priority colors, due dates, labels, projects, and subtask support.

- **Registry Key**: `todo-item`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/todo-item.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
import { TodoItem } from "@/components/ui/todo-item";

export default function Example() {
  return (
    <TodoItem
      id="1"
      title="Complete project documentation"
      description="Write comprehensive docs for the new API"
      completed={false}
      priority="high"
      dueDate={new Date()}
      onToggleComplete={(id, completed) => {
        console.log(`Todo ${id} completed: ${completed}`);
      }}
    />
  );
}
```

#### Component Source Code

##### File: `registry/new-york/ui/todo-item.tsx`

```tsx

```

---

### Tool Calls Section <a name="tool-calls-section"></a>

An expandable section showing AI agent tool usage with icons, inputs, and outputs.

- **Registry Key**: `tool-calls-section`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/tool-calls-section.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `icons`

#### How to Use

```tsx
import { ToolCallsSection } from "@/components/ui/tool-calls-section";

const toolCalls = [
  {
    tool_name: "search_web",
    tool_category: "search",
    message: "Searched for 'React best practices'",
    inputs: { query: "React best practices" },
    output: "Found 10 relevant results.",
  },
  {
    tool_name: "send_email",
    tool_category: "gmail",
    message: "Sent email to the team",
  },
];

export default function Example() {
  return <ToolCallsSection toolCalls={toolCalls} />;
}
```

#### Props & API Reference

##### `ToolCallsSection` Props

| Prop           | Type                                  | Default | Description                                   |
| -------------- | ------------------------------------- | ------- | --------------------------------------------- |
| toolCalls      | ToolCallEntry[]                       | -       | Array of tool call entries (required)         |
| integrations   | Map\<string, IntegrationInfo\>        | -       | Optional lookup for custom integration icons  |
| maxIconsToShow | number                                | 10      | Max icons in stacked display                  |
| defaultExpanded| boolean                               | false   | Start with accordion expanded                 |
| className      | string                                | -       | Custom container class                        |
| iconSize       | number                                | 21      | Icon size in pixels                           |
| renderIcon     | (call, size) => ReactNode             | -       | Custom icon renderer override                 |
| renderContent  | (content) => ReactNode                | -       | Custom content renderer for inputs/outputs    |

#### Component Source Code

##### File: `registry/new-york/ui/tool-calls-section.tsx`

```tsx
"use client";

import type { ReactNode } from "react";
import { useMemo, useState } from "react";

import { HugeiconsIcon, ArrowDown01Icon, ToolsIcon } from "@/components/icons";
import { cn } from "@/lib/utils";
import { formatToolName, getToolCategoryIcon } from "@/lib/utils/tool-icons";
import { CompactMarkdown } from "@/registry/new-york/ui/compact-markdown";

// ============================================================================
// Types
// ============================================================================

export interface ToolCallEntry {
	/** Name of the tool that was called */
	tool_name: string;
	/** Category/integration the tool belongs to (e.g., "gmail", "search", "memory") */
	tool_category: string;
	/** Human-readable message describing what the tool did */
	message?: string;
	/** Whether to show the category label (default: true) */
	show_category?: boolean;
	/** Unique ID for this tool call */
	tool_call_id?: string;
	/** Input parameters passed to the tool */
	inputs?: Record<string, unknown>;
	/** Output/result from the tool */
	output?: string;
	/** URL to custom icon for integrations */
	icon_url?: string;
	/** Friendly name for the integration (e.g., "Linear", "Slack") */
	integration_name?: string;
}

export interface IntegrationInfo {
	iconUrl?: string;
	name?: string;
}

export interface ToolCallsSectionProps {
	/** Array of tool call entries to display */
	toolCalls: ToolCallEntry[];
	/** Optional map of integration IDs to their info for icon/name lookup */
	integrations?: Map<string, IntegrationInfo>;
	/** Maximum number of icons to show in the stacked display (default: 10) */
	maxIconsToShow?: number;
	/** Whether to start with the accordion expanded (default: false) */
	defaultExpanded?: boolean;
	/** Custom class name for the container */
	className?: string;
	/** Custom icon size (default: 21) */
	iconSize?: number;
	/** Custom icon renderer override */
	renderIcon?: (call: ToolCallEntry, size: number) => ReactNode;
	/** Custom content renderer override for inputs/outputs */
	renderContent?: (content: unknown) => ReactNode;
}

// ============================================================================
// Helper Components
// ============================================================================

interface ChevronIconProps {
	isExpanded: boolean;
	size?: number;
	className?: string;
}

function ChevronIcon({ isExpanded, size = 18, className = "" }: ChevronIconProps) {
	return (
		<HugeiconsIcon
			icon={ArrowDown01Icon}
			size={size}
			className={cn(
				"transition-transform duration-200",
				isExpanded && "rotate-180",
				className,
			)}
		/>
	);
}

// ============================================================================
// Main Component
// ============================================================================

export function ToolCallsSection({
	toolCalls,
	integrations,
	maxIconsToShow = 10,
	defaultExpanded = false,
	className,
	iconSize = 21,
	renderIcon,
	renderContent,
}: ToolCallsSectionProps) {
	const [isExpanded, setIsExpanded] = useState(defaultExpanded);
	const [expandedCalls, setExpandedCalls] = useState<Set<number>>(new Set());

	// Create a lookup map for custom integrations by id
	const integrationLookup = useMemo(() => {
		if (integrations) return integrations;
		return new Map<string, IntegrationInfo>();
	}, [integrations]);

	// Helper to get icon_url with fallback to integrations lookup
	const getIconUrl = (call: ToolCallEntry): string | undefined => {
		if (call.icon_url) return call.icon_url;
		const integration = integrationLookup.get(call.tool_category);
		return integration?.iconUrl;
	};

	// Helper to get integration_name with fallback to integrations lookup
	const getIntegrationName = (call: ToolCallEntry): string | undefined => {
		if (call.integration_name) return call.integration_name;
		const integration = integrationLookup.get(call.tool_category);
		return integration?.name;
	};

	const toggleCallExpansion = (index: number) => {
		setExpandedCalls((prev) => {
			const next = new Set(prev);
			if (next.has(index)) next.delete(index);
			else next.add(index);
			return next;
		});
	};

	if (toolCalls.length === 0) return null;

	// Default icon renderer
	const defaultRenderIcon = (call: ToolCallEntry, size: number) => {
		const icon = getToolCategoryIcon(
			call.tool_category || "general",
			{ width: size, height: size },
			getIconUrl(call),
		);
		return icon || (
			<div className="p-1 min-w-8 min-h-8 bg-zinc-200 dark:bg-zinc-800 rounded-lg text-zinc-600 dark:text-zinc-400 backdrop-blur">
				<HugeiconsIcon icon={ToolsIcon} size={size} />
			</div>
		);
	};

	const iconRenderer = renderIcon || defaultRenderIcon;

	// Default content renderer
	const defaultRenderContent = (content: unknown) => (
		<CompactMarkdown content={content} />
	);

	const contentRenderer = renderContent || defaultRenderContent;

	// Render stacked rotated icons (deduplicated by category for cleaner display)
	const renderStackedIcons = () => {
		const seenCategories = new Set<string>();
		const uniqueIcons = toolCalls.filter((call) => {
			const category = call.tool_category || "general";
			if (seenCategories.has(category)) return false;
			seenCategories.add(category);
			return true;
		});
		const displayIcons = uniqueIcons.slice(0, maxIconsToShow);

		return (
			<div className="flex min-h-8 items-center -space-x-2">
				{displayIcons.map((call, index) => (
					<div
						key={`${call.tool_name}-${index}`}
						className="relative flex min-w-8 items-center justify-center"
						style={{
							rotate:
								displayIcons.length > 1
									? index % 2 === 0
										? "8deg"
										: "-8deg"
									: "0deg",
							zIndex: index,
						}}
					>
						{iconRenderer(call, iconSize)}
					</div>
				))}
				{uniqueIcons.length > maxIconsToShow && (
					<div className="z-0 flex size-7 min-h-7 min-w-7 items-center justify-center rounded-lg bg-zinc-200 dark:bg-zinc-700/60 text-xs text-zinc-600 dark:text-zinc-500 font-normal">
						+{uniqueIcons.length - maxIconsToShow}
					</div>
				)}
			</div>
		);
	};

	return (
		<div className={cn("w-fit max-w-[35rem]", className)}>
			{/* Collapsible Header */}
			<button
				type="button"
				onClick={() => setIsExpanded(!isExpanded)}
				className="flex items-center gap-2 hover:text-zinc-900 dark:hover:text-white text-zinc-500 cursor-pointer py-2"
			>
				{renderStackedIcons()}
				<span className="text-xs font-medium transition-all duration-200">
					Used {toolCalls.length} tool
					{toolCalls.length > 1 ? "s" : ""}
				</span>
				<ChevronIcon isExpanded={isExpanded} />
			</button>

			{/* Collapsible Content */}
			<div
				className={cn(
					"overflow-hidden transition-all duration-200",
					isExpanded ? "max-h-[2000px] opacity-100" : "max-h-0 opacity-0",
				)}
			>
				<div className="space-y-0 pt-1">
					{toolCalls.map((call, index) => {
						const hasCategoryText =
							call.show_category !== false &&
							call.tool_category &&
							call.tool_category !== "unknown";
						const hasDetails = call.inputs || call.output;
						const isCallExpanded = expandedCalls.has(index);

						return (
							<div
								key={`${call.tool_name}-step-${index}`}
								className="flex items-stretch gap-2"
							>
								{/* Icon column with connector line */}
								<div className="flex flex-col items-center self-stretch">
									<div className="min-h-8 min-w-8 flex items-center justify-center shrink-0">
										{iconRenderer(call, iconSize)}
									</div>
									{index < toolCalls.length - 1 && (
										<div className="w-px flex-1 bg-zinc-300 dark:bg-zinc-700 min-h-4" />
									)}
								</div>

								{/* Content column */}
								<div className="flex-1 min-w-0">
									<button
										type="button"
										className={cn(
											"flex items-center gap-1 group/parent",
											hasDetails ? "cursor-pointer" : "",
											!hasCategoryText ? "pt-2" : "",
										)}
										onClick={() => hasDetails && toggleCallExpansion(index)}
									>
										<p
											className={cn(
												"text-xs text-zinc-600 dark:text-zinc-400 font-medium",
												hasDetails && "group-hover/parent:text-zinc-900 dark:group-hover/parent:text-white",
											)}
										>
											{call.message || formatToolName(call.tool_name)}
										</p>
										{hasDetails && (
											<ChevronIcon isExpanded={isCallExpanded} size={14} />
										)}
									</button>

									{hasCategoryText && (
										<p className="text-[11px] text-zinc-400 dark:text-zinc-500 capitalize">
											{getIntegrationName(call) ||
												call.tool_category
													.replace(/_/g, " ")
													.split(" ")
													.map(
														(word) =>
															word.charAt(0).toUpperCase() +
															word.slice(1).toLowerCase(),
													)
													.join(" ")}
										</p>
									)}

									{isCallExpanded && hasDetails && (
										<div className="mt-2 space-y-2 text-[11px] bg-zinc-100 dark:bg-zinc-800/50 rounded-xl p-3 mb-3 w-fit">
											{call.inputs && Object.keys(call.inputs).length > 0 && (
												<div className="flex flex-col">
													<span className="text-zinc-400 dark:text-zinc-500 font-medium mb-1">
														Input
													</span>
													{contentRenderer(call.inputs)}
												</div>
											)}
											{call.output && (
												<div className="flex flex-col">
													<span className="text-zinc-400 dark:text-zinc-500 font-medium mb-1">
														Output
													</span>
													{contentRenderer(call.output)}
												</div>
											)}
										</div>
									)}
								</div>
							</div>
						);
					})}
				</div>
			</div>
		</div>
	);
}

export default ToolCallsSection;
```

##### File: `registry/new-york/ui/compact-markdown.tsx`

```tsx
"use client";

import type { ReactNode } from "react";

export interface CompactMarkdownProps {
	content: unknown;
	className?: string;
}

/**
 * Helper to check if a value looks like structured data (object/array/JSON)
 */
const isStructuredData = (value: unknown): boolean => {
	if (typeof value === "object" && value !== null) return true;
	if (typeof value === "string") {
		const trimmed = value.trim();
		return (
			(trimmed.startsWith("{") && trimmed.endsWith("}")) ||
			(trimmed.startsWith("[") && trimmed.endsWith("]"))
		);
	}
	return false;
};

/**
 * Try to parse and format JSON-like strings
 */
const formatJsonLikeString = (str: string): string => {
	try {
		const parsed = JSON.parse(str);
		return JSON.stringify(parsed, null, 2);
	} catch {
		// If it looks like truncated JSON, try to format it anyway
		return str;
	}
};

/**
 * Normalize content for display
 */
const normalizeValue = (content: unknown): { data: unknown; isStructured: boolean } => {
	if (typeof content === "object" && content !== null) {
		return { data: content, isStructured: true };
	}
	if (typeof content === "string") {
		const trimmed = content.trim();
		const looksLikeJson =
			(trimmed.startsWith("{") && (trimmed.endsWith("}") || trimmed.includes("}"))) ||
			(trimmed.startsWith("[") && (trimmed.endsWith("]") || trimmed.includes("]")));
		if (looksLikeJson) {
			return { data: content, isStructured: true };
		}
		return { data: content, isStructured: false };
	}
	return { data: String(content), isStructured: false };
};

/**
 * Compact display for structured data and markdown content.
 * Accepts any value and automatically formats it appropriately:
 * - Objects/Arrays: Pretty-printed JSON
 * - Strings that look like JSON: Formatted with indentation (even if truncated)
 * - Other strings: Simple text rendering or markdown if react-markdown available
 */
export function CompactMarkdown({ content, className = "" }: CompactMarkdownProps) {
	const { data, isStructured } = normalizeValue(content);

	const baseClasses = "bg-zinc-900/50 rounded-xl p-3 max-h-60 overflow-y-auto text-xs text-zinc-400 w-fit max-w-[32rem]";

	// For structured data, render as preformatted text
	if (isStructured) {
		const displayText =
			typeof data === "string"
				? formatJsonLikeString(data)
				: JSON.stringify(data, null, 2);

		return (
			<pre className={`${baseClasses} whitespace-pre-wrap break-words ${className}`}>
				{displayText}
			</pre>
		);
	}

	// For text content, render with basic formatting
	const textContent = String(data);

	return (
		<div className={`${baseClasses} leading-relaxed ${className}`}>
			{textContent}
		</div>
	);
}

export default CompactMarkdown;
```

##### File: `lib/utils/tool-icons.tsx`

```tsx
import type { IconSvgElement } from "@hugeicons/react";
import Image from "next/image";
import {
	Brain02Icon,
	CheckListIcon,
	AlarmClockIcon,
	ComputerProgramming01Icon,
	File02Icon,
	HugeiconsIcon,
	Image02Icon,
	InformationCircleIcon,
	Link01Icon,
	Notification03Icon,
	PackageOpenIcon,
	SourceCodeCircleIcon,
	SquareArrowUpRight02Icon,
	Target02Icon,
	ToolsIcon,
} from "@/components/icons";

export interface IconProps {
	size?: number;
	width?: number;
	height?: number;
	strokeWidth?: number;
	className?: string;
	color?: string;
}

// Category-specific icons with colors
export interface IconConfig {
	icon: IconSvgElement | string;
	bgColor: string;
	bgColorLight?: string; // Light mode background
	iconColor: string;
	isImage?: boolean;
}

/**
 * Normalize a category/integration name for icon lookup
 */
const normalizeCategoryName = (name: string): string => {
	if (!name) return "general";
	return name
		.toLowerCase()
		.trim()
		.replace(/[\s-]+/g, "_")
		.replace(/_+/g, "_")
		.replace(/^_|_$/g, "");
};

// Alias mapping for backwards compatibility
const iconAliases: Record<string, string> = {
	calendar: "google_calendar",
};

// Tool category icon configs - matches gaia repo pattern
const iconConfigs: Record<string, IconConfig> = {
	// Integration icons (use images)
	gmail: {
		icon: "/images/icons/gmail.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	google_calendar: {
		icon: "/images/icons/googlecalendar.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	github: {
		icon: "/images/icons/github.png",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	linear: {
		icon: "/images/icons/linear.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	slack: {
		icon: "/images/icons/slack.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	google_docs: {
		icon: "/images/icons/google_docs.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	googlesheets: {
		icon: "/images/icons/googlesheets.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	search: {
		icon: "/images/icons/google.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	weather: {
		icon: "/images/icons/weather.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	notion: {
		icon: "/images/icons/notion.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	twitter: {
		icon: "/images/icons/twitter.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	linkedin: {
		icon: "/images/icons/linkedin.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	reddit: {
		icon: "/images/icons/reddit.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	figma: {
		icon: "/images/icons/figma.svg",
		bgColor: "bg-zinc-800",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-white",
		isImage: true,
	},
	
	// Category icons (use HugeIcons components)
	todos: {
		icon: CheckListIcon,
		bgColor: "bg-emerald-500/20 backdrop-blur",
		bgColorLight: "bg-emerald-500/20",
		iconColor: "text-emerald-400",
	},
	reminders: {
		icon: AlarmClockIcon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-blue-400",
	},
	documents: {
		icon: File02Icon,
		bgColor: "bg-orange-500/20 backdrop-blur",
		bgColorLight: "bg-orange-500/20",
		iconColor: "text-orange-400",
	},
	development: {
		icon: SourceCodeCircleIcon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-cyan-400",
	},
	memory: {
		icon: Brain02Icon,
		bgColor: "bg-indigo-500/20 backdrop-blur",
		bgColorLight: "bg-indigo-500/20",
		iconColor: "text-indigo-400",
	},
	creative: {
		icon: Image02Icon,
		bgColor: "bg-pink-500/20 backdrop-blur",
		bgColorLight: "bg-pink-500/20",
		iconColor: "text-pink-400",
	},
	goal_tracking: {
		icon: Target02Icon,
		bgColor: "bg-emerald-500/20 backdrop-blur",
		bgColorLight: "bg-emerald-500/20",
		iconColor: "text-emerald-400",
	},
	notifications: {
		icon: Notification03Icon,
		bgColor: "bg-yellow-500/20 backdrop-blur",
		bgColorLight: "bg-yellow-500/20",
		iconColor: "text-yellow-400",
	},
	support: {
		icon: InformationCircleIcon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-blue-400",
	},
	general: {
		icon: InformationCircleIcon,
		bgColor: "bg-gray-500/20 backdrop-blur",
		bgColorLight: "bg-gray-500/20",
		iconColor: "text-gray-400",
	},
	integrations: {
		icon: Link01Icon,
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
	},
	
	// Agent tool call categories
	handoff: {
		icon: SquareArrowUpRight02Icon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-sky-400",
	},
	retrieve_tools: {
		icon: PackageOpenIcon,
		bgColor: "bg-indigo-500/20 backdrop-blur",
		bgColorLight: "bg-indigo-500/20",
		iconColor: "text-indigo-400",
	},
	executor: {
		icon: ComputerProgramming01Icon,
		bgColor: "bg-teal-500/20 backdrop-blur",
		bgColorLight: "bg-teal-500/20",
		iconColor: "text-teal-400",
	},
	unknown: {
		icon: ToolsIcon,
		bgColor: "bg-zinc-500/20 backdrop-blur",
		bgColorLight: "bg-zinc-500/20",
		iconColor: "text-zinc-400",
	},
};

/**
 * Get icon for a tool category with optional URL-based icon fallback.
 * Supports built-in categories and custom integration icons via iconUrl.
 */
export const getToolCategoryIcon = (
	category: string,
	iconProps: Partial<IconProps> & { showBackground?: boolean } = {},
	iconUrl?: string | null,
) => {
	const { showBackground = true, ...restProps } = iconProps;

	const defaultProps = {
		size: restProps.size || 16,
		width: restProps.width || 20,
		height: restProps.height || 20,
		strokeWidth: restProps.strokeWidth || 2,
		className: restProps.className,
	};

	// Normalize
	const normalizedCategory = normalizeCategoryName(category);

	// Resolve aliases
	const aliasedCategory =
		iconAliases[normalizedCategory] ||
		iconAliases[category] ||
		normalizedCategory;

	const finalCategory = normalizeCategoryName(aliasedCategory);

	let config = iconConfigs[finalCategory];

	// Fallback search
	if (!config) {
		const normalizedConfigs = Object.entries(iconConfigs);
		const matchingConfig = normalizedConfigs.find(
			([key]) => normalizeCategoryName(key) === finalCategory,
		);
		if (matchingConfig) {
			config = matchingConfig[1];
		}
	}

	// If no predefined config found, try iconUrl fallback for custom integrations
	if (!config) {
		if (iconUrl) {
			const iconElement = (
				<Image
					alt={`${category} Icon`}
					width={defaultProps.width}
					height={defaultProps.height}
					className={`${restProps.className || ""} aspect-square object-contain`}
					src={iconUrl}
				/>
			);
			return showBackground ? (
				<div className="rounded-lg p-1 bg-zinc-700 dark:bg-zinc-700">{iconElement}</div>
			) : (
				iconElement
			);
		}
		return null;
	}

	// Render image or component icon
	const iconElement = config.isImage ? (
		<Image
			alt={`${category} Icon`}
			width={defaultProps.width}
			height={defaultProps.height}
			className={`${restProps.className || ""} aspect-square object-contain`}
			src={config.icon as string}
		/>
	) : (
		<HugeiconsIcon
			icon={config.icon as IconSvgElement}
			size={defaultProps.size}
			className={restProps.className || config.iconColor}
		/>
	);

	// Return with or without background based on showBackground prop
	// Using dark: prefix for proper light/dark mode support
	return showBackground ? (
		<div className={`rounded-lg p-1 ${config.bgColorLight || config.bgColor} dark:${config.bgColor}`}>
			{iconElement}
		</div>
	) : (
		iconElement
	);
};

// Format tool names from snake_case to Title Case
export const formatToolName = (name: string): string => {
	return name
		.split("_")
		.map((word) => word.charAt(0).toUpperCase() + word.slice(1).toLowerCase())
		.join(" ");
};
```

---

### Twitter Card <a name="twitter-card"></a>

A Twitter/X-style post card with author info, engagement metrics, and media support.

- **Registry Key**: `twitter-card`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/twitter-card.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
import { TwitterCard } from "@/components/ui/twitter-card";

export default function Example() {
  return (
    <TwitterCard
      author={{
        name: "GAIA",
        handle: "heygaia_io",
        avatar: "/avatar.png",
        verified: true,
      }}
      content="Just shipped a new feature! 🚀"
      timestamp={new Date()}
      likes={142}
      retweets={38}
    />
  );
}
```

#### Component Source Code

##### File: `registry/new-york/ui/twitter-card.tsx`

```tsx

```

---

### Wave Spinner <a name="wave-spinner"></a>

A beautiful animated loading spinner with wave effects, multiple patterns, colors, and highly customizable animations.

- **Registry Key**: `wave-spinner`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/wave-spinner.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`

#### How to Use

##### Example 1

```tsx
import { WaveSpinner } from "@/components/ui/wave-spinner";

export default function Example() {
  return <WaveSpinner />;
}
```

##### Example 2

```tsx
import {
  WaveSpinner,
  COLOR_PRESETS,
  DELAY_PATTERNS,
  GRID_CONFIGS,
  type WaveSpinnerProps,
  type ColorPreset,
  type DelayPattern,
  type GridPattern,
} from "@/components/ui/wave-spinner";
```

##### Example 3

```tsx
<WaveSpinner aria-label="Loading user data..." />
```

##### Example 4

```tsx
// Hex color
<WaveSpinner color="#ff6b6b" />

// RGB
<WaveSpinner color="rgb(255, 107, 107)" />

// HSL
<WaveSpinner color="hsl(0, 100%, 71%)" />

// CSS variable
<WaveSpinner color="var(--my-brand-color)" />
```

##### Example 5

```tsx
import { WaveSpinner } from "@/components/ui/wave-spinner";

function SubmitButton({ isLoading, children }) {
  return (
    <button disabled={isLoading}>
      {isLoading && (
        <WaveSpinner 
          color="#ffffff" 
          size="xs" 
          pattern="line" 
        />
      )}
      {isLoading ? "Loading..." : children}
    </button>
  );
}
```

##### Example 6

```tsx
// Fast (urgent feedback)
<WaveSpinner duration={0.5} />

// Default
<WaveSpinner duration={0.7} />

// Slow (background processes)
<WaveSpinner duration={1.2} />
```

#### Props & API Reference

##### `Props` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| color | ColorPreset \| string | "primary" | Color preset name or any CSS color |
| pattern | GridPattern | "square3x3" | Grid layout pattern |
| animation | DelayPattern | "diagonalTL" | Animation delay pattern |
| duration | number | 0.7 | Animation duration in seconds |
| dotShape | "square" \| "rounded" \| "circle" | "square" | Shape of individual dots |
| size | "xs" \| "sm" \| "md" \| "lg" \| "xl" | "md" | Overall spinner size |
| className | string | - | Additional CSS classes |
| aria-label | string | "Loading" | Accessibility label |

#### Component Source Code

##### File: `registry/new-york/ui/wave-spinner.tsx`

```tsx
"use client";

import { cn } from "@/lib/utils";
import { type VariantProps, cva } from "class-variance-authority";
import type { FC } from "react";
import "./wave-spinner.css";

/**
 * WaveSpinner - A beautiful animated loading spinner with wave effects
 *
 * Features multiple patterns, colors, sizes, and animation styles.
 * Designed to be highly customizable while maintaining visual consistency.
 */

// Animation delay patterns for different visual effects
const DELAY_PATTERNS = {
  /** Diagonal wave from top-left */
  diagonalTL: [0, 0.12, 0.24, 0.12, 0.24, 0.36, 0.24, 0.36, 0.48],
  /** Diagonal wave from top-right */
  diagonalTR: [0.24, 0.12, 0, 0.36, 0.24, 0.12, 0.48, 0.36, 0.24],
  /** Diagonal wave from bottom-left */
  diagonalBL: [0.24, 0.36, 0.48, 0.12, 0.24, 0.36, 0, 0.12, 0.24],
  /** Diagonal wave from bottom-right */
  diagonalBR: [0.48, 0.36, 0.24, 0.36, 0.24, 0.12, 0.24, 0.12, 0],
  /** Ripple effect from center */
  ripple: [0.36, 0.24, 0.36, 0.24, 0, 0.24, 0.36, 0.24, 0.36],
  /** Horizontal wave */
  horizontal: [0, 0.12, 0.24, 0, 0.12, 0.24, 0, 0.12, 0.24],
  /** Vertical wave */
  vertical: [0, 0, 0, 0.12, 0.12, 0.12, 0.24, 0.24, 0.24],
  /** Random-like pattern */
  random: [0.1, 0.3, 0.2, 0.4, 0, 0.35, 0.15, 0.25, 0.45],
  /** Spiral from center */
  spiral: [0.4, 0.32, 0.24, 0.48, 0, 0.16, 0.56, 0.64, 0.08],
} as const;

// Grid configurations for different patterns
const GRID_CONFIGS = {
  /** 3x3 grid (standard) */
  square3x3: { cols: 3, count: 9 },
  /** 2x2 grid (minimal) */
  square2x2: { cols: 2, count: 4 },
  /** 4x4 grid (detailed) */
  square4x4: { cols: 4, count: 16 },
  /** Single row (linear) */
  line: { cols: 3, count: 3 },
  /** Diamond pattern */
  diamond: { cols: 3, count: 5, specialLayout: "diamond" as const },
  /** Cross pattern */
  cross: { cols: 3, count: 5, specialLayout: "cross" as const },
  /** Circle arrangement */
  circle: { cols: 1, count: 8, specialLayout: "circle" as const },
} as const;

type DelayPattern = keyof typeof DELAY_PATTERNS;
type GridPattern = keyof typeof GRID_CONFIGS;

// Color presets matching GAIA's design system
const COLOR_PRESETS = {
  /** GAIA primary blue (#00bbff) */
  primary: "#00bbff",
  /** Success green */
  success: "#22c55e",
  /** Warning amber */
  warning: "#f59e0b",
  /** Danger red */
  danger: "#ef4444",
  /** Muted zinc */
  muted: "#71717a",
  /** Purple accent */
  purple: "#a855f7",
  /** Cyan accent */
  cyan: "#06b6d4",
  /** Rose accent */
  rose: "#f43f5e",
  /** Indigo accent */
  indigo: "#6366f1",
  /** Emerald accent */
  emerald: "#10b981",
} as const;

type ColorPreset = keyof typeof COLOR_PRESETS;

// CVA variants for the container
const waveSpinnerVariants = cva("flex items-center justify-center w-fit", {
  variants: {
    size: {
      xs: "[--dot-size:3px] [--gap-size:1px]",
      sm: "[--dot-size:4px] [--gap-size:1.5px]",
      md: "[--dot-size:6px] [--gap-size:2px]",
      lg: "[--dot-size:8px] [--gap-size:3px]",
      xl: "[--dot-size:10px] [--gap-size:4px]",
    },
  },
  defaultVariants: {
    size: "md",
  },
});

// Props interface with full documentation
export interface WaveSpinnerProps extends VariantProps<
  typeof waveSpinnerVariants
> {
  /** The color of the spinner dots. Can be a preset name or any CSS color value */
  color?: ColorPreset | (string & {});
  /** The pattern of the grid layout */
  pattern?: GridPattern;
  /** The animation delay pattern for the wave effect */
  animation?: DelayPattern;
  /** Animation duration in seconds */
  duration?: number;
  /** The shape of individual dots */
  dotShape?: "square" | "rounded" | "circle";
  /** Additional className for customization */
  className?: string;
  /** Label for accessibility */
  "aria-label"?: string;
}

// Helper to get delay pattern for various grid sizes
const getDelayForIndex = (
  index: number,
  animation: DelayPattern,
  gridConfig: (typeof GRID_CONFIGS)[GridPattern],
): number => {
  const pattern = DELAY_PATTERNS[animation];

  // Special layouts use their own delay patterns
  if ("specialLayout" in gridConfig) {
    const count = gridConfig.count;
    // Create a proportional delay based on position
    const normalizedIndex = index / (count - 1);
    const patternIndex = Math.floor(normalizedIndex * (pattern.length - 1));
    return pattern[patternIndex] ?? 0;
  }

  // For larger grids, interpolate
  if (gridConfig.count > 9) {
    const gridSize = Math.sqrt(gridConfig.count);
    const row = Math.floor(index / gridSize);
    const col = index % gridSize;
    // Map to 3x3 equivalent position
    const mappedRow = Math.floor((row / gridSize) * 3);
    const mappedCol = Math.floor((col / gridSize) * 3);
    const mappedIndex = mappedRow * 3 + mappedCol;
    return pattern[Math.min(mappedIndex, pattern.length - 1)] ?? 0;
  }

  // For smaller grids, use direct mapping
  if (gridConfig.count === 4) {
    // 2x2 mapping: 0,1,3,4 of 3x3
    const mapping = [0, 2, 6, 8];
    return pattern[mapping[index] ?? 0] ?? 0;
  }

  // For 3 dots (line), use first row
  if (gridConfig.count === 3) {
    return pattern[index] ?? 0;
  }

  return pattern[index % pattern.length] ?? 0;
};

// Component to render special layouts
const SpecialLayoutDots: FC<{
  config: (typeof GRID_CONFIGS)[GridPattern];
  color: string;
  dotShape: string;
  animation: DelayPattern;
  duration: number;
}> = ({ config, color, dotShape, animation, duration }) => {
  if (!("specialLayout" in config)) return null;

  const borderRadius =
    dotShape === "circle" ? "50%" : dotShape === "rounded" ? "30%" : "0";

  if (config.specialLayout === "diamond") {
    // Diamond pattern: center + 4 corners arranged as diamond
    const positions = [
      { top: "0%", left: "50%", transform: "translateX(-50%)" },
      { top: "50%", left: "0%", transform: "translateY(-50%)" },
      { top: "50%", left: "50%", transform: "translate(-50%, -50%)" },
      { top: "50%", left: "100%", transform: "translate(-100%, -50%)" },
      { top: "100%", left: "50%", transform: "translate(-50%, -100%)" },
    ];

    return (
      <div
        className="relative"
        style={{
          width: "calc(var(--dot-size) * 3 + var(--gap-size) * 2)",
          height: "calc(var(--dot-size) * 3 + var(--gap-size) * 2)",
        }}
      >
        {positions.map((pos, idx) => (
          <div
            key={idx}
            className="absolute transition-all"
            style={{
              width: "var(--dot-size)",
              height: "var(--dot-size)",
              backgroundColor: color,
              borderRadius,
              animation: `waveSpinnerPulse ${duration}s ease-out infinite`,
              animationDelay: `${getDelayForIndex(idx, animation, config)}s`,
              ...pos,
            }}
          />
        ))}
      </div>
    );
  }

  if (config.specialLayout === "cross") {
    // Cross pattern: center + 4 edges
    const positions = [
      { top: "0%", left: "50%", transform: "translateX(-50%)" },
      { top: "50%", left: "0%", transform: "translateY(-50%)" },
      { top: "50%", left: "50%", transform: "translate(-50%, -50%)" },
      { top: "50%", left: "100%", transform: "translate(-100%, -50%)" },
      { top: "100%", left: "50%", transform: "translate(-50%, -100%)" },
    ];

    return (
      <div
        className="relative"
        style={{
          width: "calc(var(--dot-size) * 3 + var(--gap-size) * 2)",
          height: "calc(var(--dot-size) * 3 + var(--gap-size) * 2)",
        }}
      >
        {positions.map((pos, idx) => (
          <div
            key={idx}
            className="absolute transition-all"
            style={{
              width: "var(--dot-size)",
              height: "var(--dot-size)",
              backgroundColor: color,
              borderRadius,
              animation: `waveSpinnerPulse ${duration}s ease-out infinite`,
              animationDelay: `${getDelayForIndex(idx, animation, config)}s`,
              ...pos,
            }}
          />
        ))}
      </div>
    );
  }

  if (config.specialLayout === "circle") {
    // Circle arrangement: 8 dots in a circle
    const angleStep = (2 * Math.PI) / config.count;
    const radius = "calc(var(--dot-size) * 1.5)";

    return (
      <div
        className="relative"
        style={{
          width: "calc(var(--dot-size) * 4)",
          height: "calc(var(--dot-size) * 4)",
        }}
      >
        {Array.from({ length: config.count }).map((_, idx) => {
          const angle = idx * angleStep - Math.PI / 2;
          const x = Math.cos(angle);
          const y = Math.sin(angle);
          return (
            <div
              key={idx}
              className="absolute transition-all"
              style={{
                width: "var(--dot-size)",
                height: "var(--dot-size)",
                backgroundColor: color,
                borderRadius: "50%", // Always circular for circle pattern
                animation: `waveSpinnerPulse ${duration}s ease-out infinite`,
                animationDelay: `${(idx / config.count) * duration}s`,
                top: "50%",
                left: "50%",
                transform: `translate(calc(-50% + ${x} * ${radius}), calc(-50% + ${y} * ${radius}))`,
              }}
            />
          );
        })}
      </div>
    );
  }

  return null;
};

export const WaveSpinner: FC<WaveSpinnerProps> = ({
  color = "primary",
  pattern = "square3x3",
  animation = "diagonalTL",
  duration = 0.7,
  dotShape = "square",
  size,
  className,
  "aria-label": ariaLabel = "Loading",
}) => {
  // Resolve color from preset or use as-is
  const resolvedColor =
    color in COLOR_PRESETS ? COLOR_PRESETS[color as ColorPreset] : color;

  const gridConfig = GRID_CONFIGS[pattern];
  const borderRadius =
    dotShape === "circle" ? "50%" : dotShape === "rounded" ? "30%" : "0";

  // Check if this is a special layout
  const isSpecialLayout = "specialLayout" in gridConfig;

  return (
    <div
      className={cn(waveSpinnerVariants({ size }), className)}
      role="status"
      aria-label={ariaLabel}
    >
      <div className="relative flex items-center justify-center">
        {isSpecialLayout ? (
          <SpecialLayoutDots
            config={gridConfig}
            color={resolvedColor}
            dotShape={dotShape}
            animation={animation}
            duration={duration}
          />
        ) : (
          <div
            className="relative grid"
            style={{
              gridTemplateColumns: `repeat(${gridConfig.cols}, var(--dot-size))`,
              gap: "var(--gap-size)",
            }}
          >
            {Array.from({ length: gridConfig.count }).map((_, idx) => (
              <div
                key={idx}
                className="transition-all"
                style={{
                  width: "var(--dot-size)",
                  height: "var(--dot-size)",
                  backgroundColor: resolvedColor,
                  borderRadius,
                  animation: `waveSpinnerPulse ${duration}s ease-out infinite`,
                  animationDelay: `${getDelayForIndex(idx, animation, gridConfig)}s`,
                }}
              />
            ))}
          </div>
        )}
      </div>
    </div>
  );
};

// Re-export types and constants for external use
export { COLOR_PRESETS, DELAY_PATTERNS, GRID_CONFIGS };
export type { ColorPreset, DelayPattern, GridPattern };
```

##### File: `registry/new-york/ui/wave-spinner.css`

```tsx
/* Wave Spinner Animations */

@keyframes waveSpinnerPulse {
	0%,
	100% {
		opacity: 0.3;
		transform: scale(0.8);
	}
	50% {
		opacity: 1;
		transform: scale(1);
	}
}

@keyframes waveSpinnerFade {
	0%,
	100% {
		opacity: 0.2;
	}
	50% {
		opacity: 1;
	}
}

@keyframes waveSpinnerBounce {
	0%,
	100% {
		transform: translateY(0) scale(1);
		opacity: 0.6;
	}
	50% {
		transform: translateY(-25%) scale(1.1);
		opacity: 1;
	}
}

@keyframes waveSpinnerGrow {
	0%,
	100% {
		transform: scale(0.5);
		opacity: 0.4;
	}
	50% {
		transform: scale(1);
		opacity: 1;
	}
}

/* Reduced motion support */
@media (prefers-reduced-motion: reduce) {
	[role="status"] div {
		animation-duration: 0s !important;
		animation-delay: 0s !important;
		opacity: 0.7 !important;
		transform: scale(1) !important;
	}
}
```

---

### Weather Card <a name="weather-card"></a>

A beautiful, interactive weather card component with forecast, detailed metrics, and multiple weather conditions support.

- **Registry Key**: `weather-card`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/weather-card.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `icons`

#### How to Use

##### Example 1

```tsx
import { WeatherCard } from "@/components/ui/weather-card";

const weatherData = {
  coord: {
    lon: -122.4194,
    lat: 37.7749,
  },
  weather: [
    {
      id: 800,
      main: "Clear",
      description: "clear sky",
      icon: "01d",
    },
  ],
  main: {
    temp: 22,
    feels_like: 21,
    temp_min: 18,
    temp_max: 25,
    pressure: 1013,
    humidity: 60,
  },
  visibility: 10000,
  wind: {
    speed: 3.5,
    deg: 180,
  },
  dt: 1699641600,
  sys: {
    country: "US",
    sunrise: 1699622400,
    sunset: 1699660800,
  },
  timezone: -28800,
  name: "San Francisco",
  location: {
    city: "San Francisco",
    country: "United States",
    region: "California",
  },
};

export default function Example() {
  return <WeatherCard weatherData={weatherData} />;
}
```

##### Example 2

```tsx
<WeatherCard
  weatherData={weatherData}
  showForecast={false}
  showDetails={false}
/>
```

##### Example 3

```tsx
<WeatherCard weatherData={weatherData} defaultUnit="fahrenheit" />
```

##### Example 4

```tsx
<WeatherCard weatherData={weatherData} className="max-w-lg shadow-2xl" />
```

##### Example 5

```tsx
<WeatherCard
  weatherData={weatherDataWithForecast}
  showForecast={true}
  showDetails={true}
/>
```

#### Props & API Reference

##### `WeatherCard` Props

| Prop         | Type                      | Default   | Description                                    |
| ------------ | ------------------------- | --------- | ---------------------------------------------- |
| weatherData  | WeatherData               | -         | Weather data object (required)                 |
| className    | string                    | undefined | Additional CSS classes                         |
| showForecast | boolean                   | true      | Whether to show the forecast section           |
| showDetails  | boolean                   | true      | Whether to show the additional details section |
| defaultUnit  | "celsius" \| "fahrenheit" | "celsius" | Default temperature unit                       |

#### Component Source Code

##### File: `registry/new-york/ui/weather-card.tsx`

```tsx
"use client";

import type React from "react";
import { useMemo, useState } from "react";
import {
	HugeiconsIcon,
	Location01Icon,
	SunriseIcon,
	SunsetIcon,
	ThermometerIcon,
} from "@/components/icons";
import { Button } from "@/components/ui/button";
import {
	DropdownMenu,
	DropdownMenuContent,
	DropdownMenuLabel,
	DropdownMenuSeparator,
	DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import { cn } from "@/lib/utils";
import { WeatherDetailItem } from "@/registry/new-york/ui/weather-detail-item";
import {
	CloudAngledRainIcon,
	CloudAngledZapIcon,
	CloudFogIcon,
	CloudIcon,
	CloudLittleRainIcon,
	CloudSnowIcon,
	DropletIcon,
	FastWindIcon,
	Moon02Icon,
	Sun03Icon,
	Tornado02Icon,
	VisionIcon,
} from "@/registry/new-york/ui/weather-icons";

export interface WeatherLocation {
	city: string;
	country: string | null;
	region: string | null;
}

export interface WeatherMain {
	temp: number;
	feels_like: number;
	temp_min: number;
	temp_max: number;
	pressure: number;
	humidity: number;
}

export interface WeatherWind {
	speed: number;
	deg: number;
}

export interface WeatherCondition {
	id: number;
	main: string;
	description: string;
	icon: string;
}

export interface WeatherSys {
	country: string;
	sunrise: number;
	sunset: number;
}

export interface ForecastDay {
	date: string;
	timestamp: number;
	temp_min: number;
	temp_max: number;
	humidity: number;
	weather: {
		main: string;
		description: string;
		icon: string;
	};
}

export interface WeatherData {
	coord?: {
		lon: number;
		lat: number;
	};
	weather: WeatherCondition[];
	main: WeatherMain;
	visibility?: number;
	wind: WeatherWind;
	dt: number;
	sys: WeatherSys;
	timezone: number;
	name: string;
	location: WeatherLocation;
	forecast?: ForecastDay[];
}

export interface WeatherCardProps {
	weatherData: WeatherData;
	className?: string;
	showForecast?: boolean;
	showDetails?: boolean;
	defaultUnit?: "celsius" | "fahrenheit";
}

// Helper function to convert timestamp to readable time
const formatTime = (timestamp: number, timezone: number): string => {
	const date = new Date((timestamp + timezone) * 1000);
	return date.toLocaleTimeString([], { hour: "2-digit", minute: "2-digit" });
};

// Helper function to convert celsius to fahrenheit
const celsiusToFahrenheit = (celsius: number): number => {
	return (celsius * 9) / 5 + 32;
};

// Helper function to get day of week from date string
const getDayOfWeek = (dateStr: string): string => {
	const date = new Date(dateStr);
	return date.toLocaleDateString("en-US", { weekday: "long" });
};

// Helper function to get weather icon component
const getWeatherIcon = (main: string, className: string = "", fill = "") => {
	switch (main.toLowerCase()) {
		case "thunderstorm":
			return <CloudAngledZapIcon className={className} color={fill} />;
		case "drizzle":
			return <CloudLittleRainIcon className={className} color={fill} />;
		case "rain":
			return <CloudAngledRainIcon className={className} color={fill} />;
		case "snow":
			return <CloudSnowIcon className={className} color={fill} />;
		case "haze":
			return <Sun03Icon className={className} color={fill} />;
		case "mist":
		case "smoke":
		case "dust":
		case "sand":
		case "ash":
		case "squall":
			return <CloudFogIcon className={className} color={fill} />;
		case "fog":
			return <CloudFogIcon className={className} color={fill} />;
		case "tornado":
			return <Tornado02Icon className={className} color={fill} />;
		case "clear":
			return <Sun03Icon className={className} fill={fill} color={fill} />;
		case "clouds":
			return <CloudIcon className={className} color={"#E5E7EB"} />;
		default:
			return <CloudIcon className={className} color={"#E5E7EB"} />;
	}
};

export const WeatherCard: React.FC<WeatherCardProps> = ({
	weatherData,
	className,
	showForecast = true,
	showDetails = true,
	defaultUnit = "celsius",
}) => {
	const [useFahrenheit, setUseFahrenheit] = useState(
		defaultUnit === "fahrenheit",
	);
	const [forecastExpanded, setForecastExpanded] = useState(true);
	const [detailsExpanded, setDetailsExpanded] = useState(false);

	const temp = weatherData.main.temp;
	const feelsLike = weatherData.main.feels_like;
	const weatherId = weatherData.weather[0].id;
	const sunriseTime = formatTime(weatherData.sys.sunrise, weatherData.timezone);
	const sunsetTime = formatTime(weatherData.sys.sunset, weatherData.timezone);

	// Convert temperature based on selected unit
	const displayTemp = useFahrenheit
		? Math.round(celsiusToFahrenheit(temp))
		: Math.round(temp);

	const displayFeelsLike = useFahrenheit
		? Math.round(celsiusToFahrenheit(feelsLike))
		: Math.round(feelsLike);

	// Determine the weather theme based on weather conditions
	const weatherTheme = useMemo(() => {
		// weather condition codes: https://openweathermap.org/weather-conditions
		if (weatherId >= 200 && weatherId < 300) {
			return {
				name: "Thunderstorm",
				icon: <CloudAngledZapIcon className="h-16 w-16" color="#FCD34D" />,
				gradient: "bg-gradient-to-br from-slate-800/80 to-purple-900/80",
				colorCode: "#FCD34D",
			};
		} else if (weatherId >= 300 && weatherId < 400) {
			return {
				name: "Drizzle",
				icon: <CloudLittleRainIcon className="h-16 w-16" color="#93C5FD" />,
				gradient: "bg-gradient-to-br from-slate-700/80 to-blue-800/80",
				colorCode: "#93C5FD",
			};
		} else if (weatherId >= 500 && weatherId < 600) {
			return {
				name: "Rain",
				icon: <CloudAngledRainIcon className="h-16 w-16" color="#60A5FA" />,
				gradient: "bg-gradient-to-br from-slate-800/80 to-blue-900/80",
				colorCode: "#60A5FA",
			};
		} else if (weatherId >= 600 && weatherId < 700) {
			return {
				name: "Snow",
				icon: <CloudSnowIcon className="h-16 w-16" color="#E0F2FE" />,
				gradient: "bg-gradient-to-br from-blue-100/80 to-indigo-300/80",
				colorCode: "#E0F2FE",
			};
		} else if (weatherId >= 700 && weatherId < 800) {
			// Atmospheric conditions
			if (weatherId === 701) {
				return {
					name: "Mist",
					icon: <CloudFogIcon className="h-16 w-16" color="#D1D5DB" />,
					gradient: "bg-gradient-to-br from-slate-400/80 to-slate-500/80",
					colorCode: "#D1D5DB",
				};
			} else if (weatherId === 711) {
				return {
					name: "Smoke",
					icon: <CloudFogIcon className="h-16 w-16" color="#9CA3AF" />,
					gradient: "bg-gradient-to-br from-gray-500/80 to-gray-700/80",
					colorCode: "#9CA3AF",
				};
			} else if (weatherId === 721) {
				return {
					name: "Haze",
					icon: <Sun03Icon className="h-16 w-16" color="#FDE68A" />,
					gradient: "bg-gradient-to-br from-amber-300/80 to-amber-500/80",
					colorCode: "#FDE68A",
				};
			} else if (weatherId === 731 || weatherId === 761) {
				return {
					name: "Dust",
					icon: <CloudFogIcon className="h-16 w-16" color="#FEF08A" />,
					gradient: "bg-gradient-to-br from-yellow-400/80 to-yellow-600/80",
					colorCode: "#FEF08A",
				};
			} else if (weatherId === 741) {
				return {
					name: "Fog",
					icon: <CloudFogIcon className="h-16 w-16" color="#D1D5DB" />,
					gradient: "bg-gradient-to-br from-gray-400/80 to-gray-600/80",
					colorCode: "#D1D5DB",
				};
			} else if (weatherId === 751) {
				return {
					name: "Sand",
					icon: <CloudFogIcon className="h-16 w-16" color="#FDBA74" />,
					gradient: "bg-gradient-to-br from-orange-300/80 to-orange-500/80",
					colorCode: "#FDBA74",
				};
			} else if (weatherId === 762) {
				return {
					name: "Volcanic Ash",
					icon: <CloudFogIcon className="h-16 w-16" color="#D4D4D8" />,
					gradient: "bg-gradient-to-br from-zinc-600/80 to-zinc-800/80",
					colorCode: "#D4D4D8",
				};
			} else if (weatherId === 771) {
				return {
					name: "Squall",
					icon: <FastWindIcon className="h-16 w-16" color="#93C5FD" />,
					gradient: "bg-gradient-to-br from-blue-500/80 to-blue-700/80",
					colorCode: "#93C5FD",
				};
			} else if (weatherId === 781) {
				return {
					name: "Tornado",
					icon: <Tornado02Icon className="h-16 w-16" color="#CBD5E1" />,
					gradient: "bg-gradient-to-br from-slate-600/80 to-slate-900/80",
					colorCode: "#CBD5E1",
				};
			} else {
				return {
					name: "Atmosphere",
					icon: <CloudFogIcon className="h-16 w-16" color="#D1D5DB" />,
					gradient: "bg-gradient-to-br from-slate-400/80 to-slate-600/80",
					colorCode: "#D1D5DB",
				};
			}
		} else if (weatherId === 800) {
			return {
				name: "Clear",
				icon: <Sun03Icon className="h-16 w-16" color="#FBBF24" />,
				gradient: "bg-gradient-to-br from-yellow-500/80 to-orange-500/80",
				colorCode: "#FBBF24",
			};
		} else if (weatherId >= 801 && weatherId <= 802) {
			return {
				name: "Partly Cloudy",
				icon: <CloudIcon className="h-16 w-16" color="#E5E7EB" />,
				gradient: "bg-gradient-to-br from-blue-400/80 to-blue-600/80",
				colorCode: "#E5E7EB",
			};
		} else if (weatherId >= 803 && weatherId <= 804) {
			return {
				name: "Cloudy",
				icon: <CloudIcon className="h-16 w-16" color="#E5E7EB" />,
				gradient: "bg-gradient-to-br from-slate-500/80 to-slate-700/80",
				colorCode: "#E5E7EB",
			};
		} else {
			return {
				name: "Unknown",
				icon: <CloudIcon className="h-16 w-16" color="#E5E7EB" />,
				gradient: "bg-gradient-to-br from-slate-500/80 to-slate-700/80",
				colorCode: "#E5E7EB",
			};
		}
	}, [weatherId]);

	return (
		<div
			className={cn(
				"relative w-full overflow-hidden rounded-3xl p-6 shadow-lg backdrop-blur-sm sm:max-w-md",
				weatherTheme.gradient,
				className,
			)}
		>
			{/* Location Info */}
			<div className="mb-3 flex items-start justify-between gap-10">
				<div className="flex items-start">
					<HugeiconsIcon
						icon={Location01Icon}
						size={20}
						className="relative top-1 mr-2 text-white"
					/>
					<div>
						<h2 className="flex items-center text-xl font-bold text-white">
							{weatherData.location.city}
							{weatherData.location.region
								? `, ${weatherData.location.region}`
								: ""}
						</h2>
						<p className="text-xs" style={{ color: weatherTheme.colorCode }}>
							{weatherData.location.country}
						</p>
					</div>
				</div>

				<div className="flex items-center">
					<DropdownMenu>
						<DropdownMenuTrigger asChild>
							<Button
								variant="ghost"
								size="icon"
								className="h-8 w-8 rounded-full bg-white/10 text-white hover:bg-white/20 hover:text-white"
								aria-label="Temperature settings"
							>
								<HugeiconsIcon icon={ThermometerIcon} size={20} />
							</Button>
						</DropdownMenuTrigger>
						<DropdownMenuContent
							align="end"
							className="w-40 border-zinc-700 bg-zinc-800 text-white"
						>
							<DropdownMenuLabel>Temperature Unit</DropdownMenuLabel>
							<DropdownMenuSeparator className="bg-zinc-700" />
							<div className="px-2 py-2">
								<div className="flex items-center justify-between gap-2">
									<Button
										variant={useFahrenheit ? "secondary" : "ghost"}
										size="sm"
										className="flex-1"
										onClick={() => setUseFahrenheit(true)}
									>
										°F
									</Button>
									<Button
										variant={!useFahrenheit ? "secondary" : "ghost"}
										size="sm"
										className="flex-1"
										onClick={() => setUseFahrenheit(false)}
									>
										°C
									</Button>
								</div>
							</div>
						</DropdownMenuContent>
					</DropdownMenu>
				</div>
			</div>

			{/* Main Weather Display */}
			<div className="mb-2 flex items-center justify-between gap-5">
				<div className="flex items-center justify-center">
					{weatherTheme.icon}
				</div>

				<div>
					<div className="flex items-baseline">
						<span className="text-4xl font-bold text-white">
							{displayTemp}°
						</span>
						<span className="ml-1 text-sm font-medium text-white/80">
							{useFahrenheit ? "F" : "C"}
						</span>
					</div>
					<p
						className="text-xs"
						style={{
							color: weatherTheme.colorCode,
							filter: "brightness(1.3)",
						}}
					>
						Feels like: {displayFeelsLike}°
					</p>
				</div>

				<p className="capitalize font-medium text-white">
					{weatherData.weather[0].description}
				</p>
			</div>

			{/* Weather Forecast */}
			{showForecast &&
				weatherData.forecast &&
				weatherData.forecast.length > 0 && (
					<div className="mt-4">
						<button
							type="button"
							onClick={() => setForecastExpanded(!forecastExpanded)}
							className="mb-2 flex w-full items-center justify-between text-sm font-normal text-white"
						>
							<span>Weekly Forecast</span>
							<svg
								className={cn(
									"h-4 w-4 transition-transform",
									forecastExpanded && "rotate-180",
								)}
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
								aria-hidden="true"
							>
								<path
									strokeLinecap="round"
									strokeLinejoin="round"
									strokeWidth={2}
									d="M19 9l-7 7-7-7"
								/>
							</svg>
						</button>
						{forecastExpanded && (
							<div className="space-y-2 pb-2">
								{weatherData.forecast.map((day, idx) => {
									const dayTemp = useFahrenheit
										? Math.round(celsiusToFahrenheit(day.temp_max))
										: Math.round(day.temp_max);
									const nightTemp = useFahrenheit
										? Math.round(celsiusToFahrenheit(day.temp_min))
										: Math.round(day.temp_min);

									return (
										<div
											key={idx}
											className="flex items-center justify-start rounded-2xl bg-black/10 px-2 py-1 text-white"
										>
											<div className="flex w-full flex-1 items-center justify-start gap-2">
												<div className="flex items-center justify-center">
													{getWeatherIcon(
														day.weather.main,
														"h-7 w-7",
														weatherTheme.colorCode,
													)}
												</div>
												<div className="w-24">
													<span className="font-semibold text-white">
														{getDayOfWeek(day.date)}
													</span>
												</div>
											</div>

											<div className="flex w-24 justify-end">
												<div className="flex flex-row items-end gap-2">
													<div className="flex items-center">
														<Sun03Icon
															className="mr-1.5 h-7 w-7"
															color="#FCD34D"
															fill="#FCD34D"
														/>
														<span className="w-8 font-medium text-white">
															{dayTemp}°
														</span>
													</div>
													<div className="mt-1 flex items-center">
														<Moon02Icon
															className="mr-1.5 h-7 w-7"
															color="#93C5FD"
															fill="#93C5FD"
														/>
														<span className="w-8 text-white/80">
															{nightTemp}°
														</span>
													</div>
												</div>
											</div>
										</div>
									);
								})}
							</div>
						)}
					</div>
				)}

			{/* Weather Details */}
			{showDetails && (
				<div className="mt-4">
					<button
						type="button"
						onClick={() => setDetailsExpanded(!detailsExpanded)}
						className="mb-2 flex w-full items-center justify-between text-sm font-normal text-white"
					>
						<span>Additional Information</span>
						<svg
							className={cn(
								"h-4 w-4 transition-transform",
								detailsExpanded && "rotate-180",
							)}
							fill="none"
							stroke="currentColor"
							viewBox="0 0 24 24"
							aria-hidden="true"
						>
							<path
								strokeLinecap="round"
								strokeLinejoin="round"
								strokeWidth={2}
								d="M19 9l-7 7-7-7"
							/>
						</svg>
					</button>
					{detailsExpanded && (
						<div className="mt-2 grid grid-cols-3 gap-2">
							{[
								{
									icon: (
										<FastWindIcon
											className="h-6 w-6"
											color={weatherTheme.colorCode}
										/>
									),
									label: "Wind",
									value: `${weatherData.wind.speed} m/s`,
									tooltipText: "Wind speed in meters per second",
								},
								{
									icon: (
										<DropletIcon
											className="h-6 w-6"
											color={weatherTheme.colorCode}
										/>
									),
									label: "Humidity",
									value: `${weatherData.main.humidity}%`,
									tooltipText: "Amount of water vapor in the air",
								},
								...(weatherData.visibility
									? [
											{
												icon: (
													<VisionIcon
														className="h-6 w-6"
														color={weatherTheme.colorCode}
													/>
												),
												label: "Visibility",
												value: `${(weatherData.visibility / 1000).toFixed(
													1,
												)} km`,
												tooltipText: "Maximum visibility distance",
											},
										]
									: []),
								{
									icon: (
										<CloudIcon
											className="h-6 w-6"
											color={weatherTheme.colorCode}
										/>
									),
									label: "Pressure",
									value: `${weatherData.main.pressure} hPa`,
									tooltipText: "Atmospheric pressure in hectopascals",
								},
								{
									icon: (
										<HugeiconsIcon
											icon={SunriseIcon}
											size={24}
											color={weatherTheme.colorCode}
										/>
									),
									label: "Sunrise",
									value: sunriseTime,
									tooltipText: "Time when the sun rises above the horizon",
								},
								{
									icon: (
										<HugeiconsIcon
											icon={SunsetIcon}
											size={24}
											color={weatherTheme.colorCode}
										/>
									),
									label: "Sunset",
									value: sunsetTime,
									tooltipText: "Time when the sun disappears below the horizon",
								},
							].map((detail, index) => (
								<WeatherDetailItem
									key={index}
									icon={detail.icon}
									label={detail.label}
									value={detail.value}
									tooltipText={detail.tooltipText}
									highlight={weatherTheme.colorCode}
								/>
							))}
						</div>
					)}
				</div>
			)}
		</div>
	);
};
```

##### File: `registry/new-york/ui/weather-detail-item.tsx`

```tsx
"use client";

import type React from "react";
import type { ReactNode } from "react";
import {
	Tooltip,
	TooltipContent,
	TooltipProvider,
	TooltipTrigger,
} from "@/components/ui/tooltip";

export interface WeatherDetailItemProps {
	icon: ReactNode;
	label: string;
	value: string;
	tooltipText?: string;
	highlight: string;
}

export const WeatherDetailItem: React.FC<WeatherDetailItemProps> = ({
	icon,
	label,
	value,
	tooltipText,
	highlight,
}) => {
	return (
		<div className="flex flex-col items-start rounded-2xl bg-black/10 p-2 px-3 backdrop-blur-sm">
			<TooltipProvider>
				<Tooltip>
					<TooltipTrigger asChild>
						<div className="flex w-full cursor-help flex-row items-start justify-between">
							<div className="flex flex-col">
								<div className="w-fit text-sm text-white/70">{label}</div>
								<div className="w-fit font-medium text-white">{value}</div>
							</div>
							<div style={{ color: highlight }}>{icon}</div>
						</div>
					</TooltipTrigger>
					{tooltipText && (
						<TooltipContent>
							<p>{tooltipText}</p>
						</TooltipContent>
					)}
				</Tooltip>
			</TooltipProvider>
		</div>
	);
};
```

##### File: `registry/new-york/ui/weather-icons.tsx`

```tsx
import type React from "react";

export interface IconProps extends React.SVGProps<SVGSVGElement> {
	color?: string;
}

export const CloudAngledZapIcon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Storm"
		role="img"
		{...props}
	>
		<path
			d="M7 17.5C4.23858 17.5 2 15.336 2 12.6667C2 10.1537 3.98398 8.0886 6.52042 7.85528M17.5 17.5C19.9853 17.5 22 15.5524 22 13.15C22 10.7476 19.9853 8.8 17.5 8.8C17.4925 8.8 17.485 8.80002 17.4776 8.80005M17.4776 8.80005C17.4924 8.64084 17.5 8.47961 17.5 8.31667C17.5 5.38035 15.0376 3 12 3C9.12324 3 6.76233 5.135 6.52042 7.85528M17.4776 8.80005C17.3753 9.89668 16.9286 10.8973 16.2428 11.7M6.52042 7.85528C6.67826 7.84076 6.83823 7.83333 7 7.83333C8.12582 7.83333 9.16474 8.19302 10.0005 8.8"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
		<path
			d="M12.5784 14L10.8043 16.6838C10.5668 17.0431 10.4481 17.2227 10.5217 17.3614C10.5952 17.5 10.8093 17.5 11.2375 17.5H12.7625C13.1907 17.5 13.4048 17.5 13.4783 17.6386C13.5519 17.7773 13.4332 17.9569 13.1957 18.3162L11.4216 21"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
	</svg>
);

export const CloudLittleRainIcon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Light Rain"
		role="img"
		{...props}
	>
		<path
			d="M12.0011 13.5V15M9 16.5V18M15 16.5V18M6.5 19.5V21M17.5 19.5V21M12 19.5V21"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
		<path
			d="M17.4776 8.89801L17.5 8.89795C19.9853 8.89795 22 10.8784 22 13.3214C22 14.8551 21.206 16.2065 20 17M17.4776 8.89801C17.4924 8.73611 17.5 8.57216 17.5 8.40646C17.5 5.42055 15.0376 3 12 3C9.12324 3 6.76233 5.17106 6.52042 7.93728M17.4776 8.89801C17.3753 10.0132 16.9286 11.0307 16.2428 11.8469M6.52042 7.93728C3.98398 8.17454 2 10.2745 2 12.8299C2 14.4378 2.78565 15.8652 4 16.7619M6.52042 7.93728C6.67826 7.92251 6.83823 7.91496 7 7.91496C8.12582 7.91496 9.16474 8.28072 10.0005 8.89795"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
	</svg>
);

export const CloudAngledRainIcon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Rain"
		role="img"
		{...props}
	>
		<path
			d="M12.5 15L11.5 17M17 15L16 17M13.5 19L12.5 21M8 15L7 17M9 19L8 21"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
		<path
			d="M17.4776 8.89801L17.5 8.89795C19.9853 8.89795 22 10.8784 22 13.3214C22 14.8551 21.206 16.2065 20 17M17.4776 8.89801C17.4924 8.73611 17.5 8.57216 17.5 8.40646C17.5 5.42055 15.0376 3 12 3C9.12324 3 6.76233 5.17106 6.52042 7.93728M17.4776 8.89801C17.3753 10.0132 16.9286 11.0307 16.2428 11.8469M6.52042 7.93728C3.98398 8.17454 2 10.2745 2 12.8299C2 14.4378 2.78565 15.8652 4 16.7619M6.52042 7.93728C6.67826 7.92251 6.83823 7.91496 7 7.91496C8.12582 7.91496 9.16474 8.28072 10.0005 8.89795"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
	</svg>
);

export const CloudSnowIcon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Snow"
		role="img"
		{...props}
	>
		<path
			d="M17.4776 8.7803L17.5 8.78025C19.9853 8.78025 22 10.7212 22 13.1154C22 14.8176 20.9817 16.2906 19.5 17M17.4776 8.7803C17.4924 8.62164 17.5 8.46095 17.5 8.29856C17.5 5.37225 15.0376 3 12 3C9.12324 3 6.76233 5.12773 6.52042 7.83875M17.4776 8.7803C17.3753 9.8732 16.9286 10.8704 16.2428 11.6704M6.52042 7.83875C3.98398 8.07128 2 10.1293 2 12.6338C2 14.566 3.18102 16.2326 4.88559 17M6.52042 7.83875C6.67826 7.82428 6.83823 7.81688 7 7.81688C8.12582 7.81688 9.16474 8.17534 10.0005 8.78025"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
		<path
			d="M11.9978 16.9974L12.0022 17.0052M15.9955 15L16 15.0078M8 15L8.00449 15.0078M15.9955 18.9948L16 19.0026M8 18.9948L8.00449 19.0026M11.9978 20.9922L12.0022 21"
			stroke="currentColor"
			strokeWidth="2"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
	</svg>
);

export const CloudFogIcon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Fog"
		role="img"
		{...props}
	>
		<path
			d="M17.4776 9.00005C17.485 9.00002 17.4925 9 17.5 9C19.9853 9 22 11.0147 22 13.5C22 15.9853 19.9853 18 17.5 18H7C4.23858 18 2 15.7614 2 13C2 10.4003 3.98398 8.26407 6.52042 8.0227M17.4776 9.00005C17.4924 8.83536 17.5 8.66856 17.5 8.5C17.5 5.46243 15.0376 3 12 3C9.12324 3 6.76233 5.20862 6.52042 8.0227M17.4776 9.00005C17.3753 10.1345 16.9286 11.1696 16.2428 12M6.52042 8.0227C6.67826 8.00768 6.83823 8 7 8C8.12582 8 9.16474 8.37209 10.0005 9"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
		<path
			d="M6 21H8"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
		<path
			d="M11 21H13"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
		<path
			d="M16 21H18"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
	</svg>
);

export const CloudIcon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Cloud"
		role="img"
		{...props}
	>
		<path
			d="M17.4776 10.0001C17.485 10 17.4925 10 17.5 10C19.9853 10 22 12.0147 22 14.5C22 16.9853 19.9853 19 17.5 19H7C4.23858 19 2 16.7614 2 14C2 11.4003 3.98398 9.26407 6.52042 9.0227M17.4776 10.0001C17.4924 9.83536 17.5 9.66856 17.5 9.5C17.5 6.46243 15.0376 4 12 4C9.12324 4 6.76233 6.20862 6.52042 9.0227M17.4776 10.0001C17.3753 11.1345 16.9286 12.1696 16.2428 13M6.52042 9.0227C6.67826 9.00768 6.83823 9 7 9C8.12582 9 9.16474 9.37209 10.0005 10"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
	</svg>
);

export const FastWindIcon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Wind"
		role="img"
		{...props}
	>
		<path
			d="M2 5.94145C5.5 9.37313 10.5755 7.90241 11.7324 5.94145C11.9026 5.65301 12 5.31814 12 4.96096C12 3.87795 11.1046 3 10 3C8.89543 3 8 3.87795 8 4.96096"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
		<path
			d="M17 8.92814C17 7.31097 18.1193 6 19.5 6C20.8807 6 22 7.31097 22 8.92814C22 9.6452 21.7799 10.3021 21.4146 10.8111C19.3463 14.1915 9.2764 12.9164 4 11.8563"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
		<path
			d="M13.0854 19.8873C13.2913 20.5356 13.8469 21 14.5 21C15.3284 21 16 20.2528 16 19.331C16 19.0176 15.9224 18.7244 15.7873 18.4738C14.4999 15.9925 7.99996 14.3239 2 18.7746"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
		<path
			d="M19 15.5H21"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
	</svg>
);

export const Sun03Icon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Sun"
		role="img"
		{...props}
	>
		<path
			d="M17 12C17 14.7614 14.7614 17 12 17C9.23858 17 7 14.7614 7 12C7 9.23858 9.23858 7 12 7C14.7614 7 17 9.23858 17 12Z"
			stroke="currentColor"
			strokeWidth="1.5"
		/>
		<path
			d="M12 2V3.5M12 20.5V22M19.0708 19.0713L18.0101 18.0106M5.98926 5.98926L4.9286 4.9286M22 12H20.5M3.5 12H2M19.0713 4.92871L18.0106 5.98937M5.98975 18.0107L4.92909 19.0714"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
	</svg>
);

export const Moon02Icon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Moon"
		role="img"
		{...props}
	>
		<path
			d="M21.5 14.0784C20.3003 14.7189 18.9301 15.0821 17.4751 15.0821C12.7491 15.0821 8.91792 11.2509 8.91792 6.52485C8.91792 5.06986 9.28105 3.69968 9.92163 2.5C5.66765 3.49698 2.5 7.31513 2.5 11.8731C2.5 17.1899 6.8101 21.5 12.1269 21.5C16.6849 21.5 20.503 18.3324 21.5 14.0784Z"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
	</svg>
);

export const Tornado02Icon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Tornado"
		role="img"
		{...props}
	>
		<path
			d="M9.01654 6.15879C10.9944 4.77262 17.9171 3.55944 18.906 6.15879C20.3862 10.0497 3.87743 12.3805 5.06077 6.15849C5.55508 3.55944 10.5002 2 12.9725 2"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
		<path
			d="M18 11.1934C15.423 13.0706 8.5771 13.8244 6 11.7816"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
		<path
			d="M14.0219 21.6941C13.0436 21.8816 12.0077 21.989 11 21.9995"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
		<path
			d="M7 15.166C9.07731 16.0444 12.3835 15.9574 15 15.2812"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
		<path
			d="M8.03906 18.5039C9.49304 18.8598 11.2867 18.8854 12.9988 18.6635"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
		/>
	</svg>
);

export const DropletIcon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Droplet"
		role="img"
		{...props}
	>
		<path
			d="M3.5 13.678C3.5 9.49387 7.08079 5.35907 9.59413 2.97222C10.9591 1.67593 13.0409 1.67593 14.4059 2.97222C16.9192 5.35907 20.5 9.49387 20.5 13.678C20.5 17.7804 17.2812 22 12 22C6.71878 22 3.5 17.7804 3.5 13.678Z"
			stroke="currentColor"
			strokeWidth="1.5"
		/>
		<path
			d="M16 14C16 16.2091 14.2091 18 12 18"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
	</svg>
);

export const VisionIcon: React.FC<IconProps> = (props) => (
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 24 24"
		width={24}
		height={24}
		color={"#000000"}
		fill={"none"}
		aria-label="Vision"
		role="img"
		{...props}
	>
		<path
			d="M2.5 8.18677C2.60406 6.08705 2.91537 4.77792 3.84664 3.84664C4.77792 2.91537 6.08705 2.60406 8.18677 2.5M21.5 8.18677C21.3959 6.08705 21.0846 4.77792 20.1534 3.84664C19.2221 2.91537 17.9129 2.60406 15.8132 2.5M15.8132 21.5C17.9129 21.3959 19.2221 21.0846 20.1534 20.1534C21.0846 19.2221 21.3959 17.9129 21.5 15.8132M8.18676 21.5C6.08705 21.3959 4.77792 21.0846 3.84664 20.1534C2.91537 19.2221 2.60406 17.9129 2.5 15.8132"
			stroke="currentColor"
			strokeWidth="1.5"
			strokeLinecap="round"
			strokeLinejoin="round"
		/>
		<path
			d="M19.6352 11.3178C19.8784 11.6224 20 11.7746 20 12C20 12.2254 19.8784 12.3776 19.6352 12.6822C18.5423 14.0504 15.7514 17 12 17C8.24862 17 5.45768 14.0504 4.36483 12.6822C4.12161 12.3776 4 12.2254 4 12C4 11.7746 4.12161 11.6224 4.36483 11.3178C5.45768 9.9496 8.24862 7 12 7C15.7514 7 18.5423 9.9496 19.6352 11.3178Z"
			stroke="currentColor"
			strokeWidth="1.5"
		/>
		<path
			d="M14 12C14 10.8954 13.1046 10 12 10C10.8954 10 10 10.8954 10 12C10 13.1046 10.8954 14 12 14C13.1046 14 14 13.1046 14 12Z"
			stroke="currentColor"
			strokeWidth="1.5"
		/>
	</svg>
);
```

---

### Workflow Card <a name="workflow-card"></a>

A card component for displaying workflow automations with rotated tool icons, execution stats, and action buttons.

- **Registry Key**: `workflow-card`

#### Installation

```bash
npx shadcn@latest add https://ui.heygaia.io/r/workflow-card.json
```

#### Dependencies

- **NPM Packages**:
  - `lucide-react`

#### How to Use

```tsx
import { WorkflowCard } from "@/components/ui/workflow-card";

export default function Example() {
  return (
    <WorkflowCard
      title="Email Summary Workflow"
      description="Summarize your daily emails automatically"
      steps={[
        { id: "1", title: "Fetch emails", toolCategory: "email" },
        { id: "2", title: "Summarize", toolCategory: "documents" },
      ]}
      totalExecutions={1250}
      isActivated={true}
      onAction={() => console.log("Run workflow")}
    />
  );
}
```

#### Props & API Reference

##### `WorkflowCard` Props

| Prop | Type | Default | Description |
| --- | --- | --- | --- |
| title | string | - | Workflow title |
| description | string | - | Workflow description |
| steps | WorkflowStep[] | - | Array of workflow steps |
| totalExecutions | number | 0 | Total execution count |
| isActivated | boolean | false | Activation status (user variant) |
| triggerLabel | string | - | Trigger description |
| variant | "user" \| "explore" \| "suggestion" | "explore" | Card variant |
| actionLabel | string | - | Custom action button label |
| isLoading | boolean | false | Show loading spinner |
| onAction | () => void | - | Action button handler |
| onClick | () => void | - | Card click handler |

#### Component Source Code

##### File: `registry/new-york/ui/workflow-card.tsx`

```tsx

```

---
