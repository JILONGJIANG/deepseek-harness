# next-turn should queue, not reject

`followup` declares mode `next-turn`, which means "run on the next turn". It
should queue. It rejects instead.

`packages/core/agent-loop/src/agent.ts:163` calls
`this.send(input, 'next-turn', true)` directly. While a turn is running that
throws, and `packages/api/session-controller/src/commands.ts` wraps the throw as
`session/agent-busy`. No configuration makes it queue. There is no switch left
off; the switch does not exist.

TODO(明早): change the busy branch of `send` so a `next-turn` followup is
enqueued and acknowledged instead of thrown. Do not change the logic tonight.
