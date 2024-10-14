# singlend

Multiple operations on a single endpoint with zod 🚀

## Hoe to use

```bash
npx jsr add @evex/singlend
bunx jsr add @evex/singlend
deno add @evex/singlend
```

```ts
import { Hono } from "@hono/hono";
import { z } from "zod";
import { Singlend } from "@evex/singlend";

const app = new Hono();
const singlend = new Singlend();

singlend
	.on(
		"setIcon",
		z.object({
			iconUrl: z.string(),
		}),
		(query, ok) =>
			ok({
				message: "Set icon to " + query.iconUrl,
			}),
	)
	.on(
		"setBackgroundColor",
		z.object({
			backgroundColor: z.string().length(7),
		}),
		(query, ok, error) => {
			if (!query.backgroundColor.startsWith("#")) {
				return error({
					message: "Invalid background color",
				});
			}

			return ok({
				message: "Set background color to " + query.backgroundColor,
			});
		},
	);

app.use("/api/singlend", singlend.middleware());
```
