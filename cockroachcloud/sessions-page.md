---
title: Sessions Page
summary: The Sessions page provides details of all open sessions in the cluster.
toc: true
---

The **Sessions** page of the {{ site.data.products.db }} Console provides details of all open sessions in the cluster.

To view this page, click **Sessions** in the left-hand navigation of the {{ site.data.products.db }} Console.

## Sessions list

Use the **Sessions** list to see the open sessions in the cluster. This includes active and idle sessions.

{{site.data.alerts.callout_info}}
A session is *active* if it has an open transaction (including implicit transactions, which are individual SQL statements), and *idle* if it has no open transaction. Active sessions consume hardware resources.
{{site.data.alerts.end}}

- If a session is active, the most recent SQL statement is displayed in the **Statement** column.
- If a session is idle, **Transaction Duration**, **Statement Duration**, and **Statement** will display `N/A`.
- To view [details of a session](#session-details), click the **Session Duration**.

{{site.data.alerts.callout_info}}
An active session can have an open transaction that is not currently running SQL. In this case, the **Statement** and **Statement Duration** columns will display `N/A` and **Transaction Duration** will display a value. Transactions that are held open can cause [contention](../{{site.versions["stable"]}}/performance-best-practices-overview.html#understanding-and-avoiding-transaction-contention).
{{site.data.alerts.end}}

<img src="{{ 'images/cockroachcloud/sessions-page.png' | relative_url }}" alt="{{ site.data.products.db }} Console Database Tables View" style="border:1px solid #eee;max-width:100%" />

The following are displayed for each active session:

Parameter | Description
--------- | -----------
Session Duration | Amount of time the session has been open.
Transaction Duration | Amount of time the transaction has been active, if there is an open transaction.
Statement Duration | Amount of time the SQL statement has been active, if there is an active statement.
Memory Usage | Amount of memory currently allocated to this session, followed by the maximum amount of memory this session has ever been allocated.
Statement | Active SQL statement. If more than one statement is active, the most recent statement is shown.
Actions | Options to terminate the active query and/or terminate the session. These require the [`CANCELQUERY` role option](../{{site.versions["stable"]}}/authorization.html#create-and-manage-users).<br><br>**Terminate Statement:** Ends the SQL statement. The session running this statement will receive an error.<br><br>**Terminate Session:** Ends the session. The client that holds this session will receive a "connection terminated" event.

{{site.data.alerts.callout_success}}
Sort by **Transaction Duration** to display all active sessions at the top.
{{site.data.alerts.end}}

## Session details

Click the **Session Duration** of any session to display details and possible actions for that session.

<img src="{{ 'images/cockroachcloud/sessions-details-page.png' | relative_url }}" alt="{{ site.data.products.db }} Console Database Tables View" style="border:1px solid #eee;max-width:100%" />

- **Session** shows the ID of the connected session.
	- **Session Start Time** shows the timestamp at which the session started.
	- **Gateway Node** shows the node ID and IP address/port of the [gateway](../{{site.versions["stable"]}}/architecture/life-of-a-distributed-transaction.html#gateway) node handling the client connection.
	- **Client Address** shows the IP address/port of the client that opened the session.
	- **Memory Usage** shows the amount of memory currently allocated to this session, followed by the maximum amount of memory this session has ever allocated.
	- The **Terminate Session** button ends the session. The client that holds this session will receive a "connection terminated" event.
	- The **Terminate Statement** button ends the SQL statement. The session running this statement will receive an error.
- **Transaction** will display the following information for an open transaction.
	- **Transaction Start Time** shows the timestamp at which the transaction started.
	- **Number of Statements Executed** shows the total number of SQL statements executed by the transaction.
	- **Number of Retries** shows the total number of [retries](../{{site.versions["stable"]}}/transactions.html#transaction-retries) for the transaction.
	- **Number of Automatic Retries** shows the total number of [automatic retries](../{{site.versions["stable"]}}/transactions.html#automatic-retries) run by CockroachDB for the transaction.
	- **Priority** shows the [priority](../{{site.versions["stable"]}}/transactions.html#transaction-priorities) for the transaction.
	- **Read Only?** shows whether the transaction is read-only.
	- **AS OF SYSTEM TIME?** shows whether the transaction uses [`AS OF SYSTEM TIME`](../{{site.versions["stable"]}}/performance-best-practices-overview.html#use-as-of-system-time-to-decrease-conflicts-with-long-running-queries) to return historical data.
	- **Memory Usage** shows the amount of memory currently allocated to this transaction, followed by the maximum amount of memory this transaction has ever allocated.
- **Statement** will display the following information for an active statement.
	- The SQL statement is shown.
	- **Execution Start Time** shows the timestamp at which the statement was run.
	- **Distributed Execution?** shows whether the statement uses [Distributed SQL (DistSQL)](../{{site.versions["stable"]}}/architecture/sql-layer.html#distsql) optimization.
	- **View Statement Details** opens the [Statement Details](statements-page.html#statement-details-page) page for the statement.
