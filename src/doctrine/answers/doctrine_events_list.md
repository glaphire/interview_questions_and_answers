# List of Doctrine Events

[lib/Doctrine/ORM/Events.php on github](https://github.com/doctrine/orm/blob/2.8.x/lib/Doctrine/ORM/Events.php)

<table border=1 cellpadding=5>
	<tr>
		<td colspan=1>Doctrine Events</td>
	</tr>
	<tr>
		<td>Event name</td>
		<td>Lifecycle part of</td>
		<td>Description</td>
	</tr>
	<tr>
		<td>preRemove</td>
		<td>Part of Entity Lifecycle</td>
		<td> The <b>preRemove</b> event occurs for a given entity before the respective 
		EntityManager remove operation for that entity is executed.</td>
	</tr>
	<tr>
		<td>postRemove</td>
		<td>Entity</td>
		<td>The <b>postRemove</b> event occurs for an entity after the entity has been deleted. 
		It will be invoked after the database delete operations.</td>
	</tr>
	<tr>
		<td>prePersist</td>
		<td>Entity</td>
		<td>The <b>prePersist</b> event occurs for a given entity before 
		the respective EntityManager persist operation for that entity is executed.</td>
	</tr>
	<tr>
		<td>postPersist</td>
		<td>Entity</td>
		<td>The <b>postPersist</b> event occurs for an entity after the entity has
            been made persistent. It will be invoked after the database insert operations.
            Generated primary key values are available in the postPersist event.</td>
	</tr>
	<tr>
		<td>preUpdate</td>
		<td>Entity</td>
		<td>The preUpdate event occurs before the database update operations to entity data.</td>
	</tr>
	<tr>
		<td>postUpdate</td>
		<td>Entity</td>
		<td>The postUpdate event occurs after the database update operations to entity data.</td>
	</tr>
	<tr>
		<td>postLoad</td>
		<td>Entity</td>
		<td>The <b>postLoad</b> event occurs for an entity after the entity has been loaded
			into the current EntityManager from the database or after the refresh operation
			has been applied to it. <i>Note that the postLoad event occurs for an entity before 
			any associations have been initialized.</i> 
			Therefore it is not safe to access associations in a postLoad callback
            or event handler.
            </td>
	</tr>
	<tr>
		<td>loadClassMetadata</td>
		<td>Bootstraping</td>
		<td>The <b>loadClassMetadata</b> event occurs after the mapping metadata for a class
			has been loaded from a mapping source (annotations/xml/yaml).</td>
	</tr>
	<tr>
		<td>onClassMetadataNotFound</td>
		<td>Bootstraping</td>
		<td>The <b>onClassMetadataNotFound</b> event occurs whenever loading metadata for a class failed.</td>
	</tr>
	<tr>
		<td>preFlush</td>
		<td>UnitOfWork</td>
		<td>The <b>preFlush</b> event occurs when the EntityManager#flush() operation is invoked,
			but before any changes to managed entities have been calculated. This event is
			always raised right after EntityManager#flush() call.</td>
	</tr>
	<tr>
		<td>OnFlush</td>
		<td>UnitOfWork</td>
		<td>The <b>onFlush</b> event occurs when the EntityManager#flush() operation is invoked,
			after any changes to managed entities have been determined but before any
			actual database operations are executed. The event is only raised if there is
			actually something to do for the underlying UnitOfWork. If nothing needs to be done,
			the onFlush event is not raised.</td>
	</tr>
	<tr>
		<td>postFlush</td>
		<td>UnitfOfWork</td>
		<td>The <b>postFlush</b> event occurs when the EntityManager#flush() operation is invoked and
		after all actual database operations are executed successfully. The event is only raised if there is
        actually something to do for the underlying UnitOfWork. If nothing needs to be done,
        the postFlush event is not raised. The event won't be raised if an error occurs during the
        flush operation.</td>
	</tr>
	<tr>
		<td>onClear</td>
		<td>UnifOfWork</td>
		<td>The onClear event occurs when the EntityManager#clear() operation is invoked,
        after all references to entities have been removed from the unit of work.</td>
	</tr>
</table>
