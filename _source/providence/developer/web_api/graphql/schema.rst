Schema information (endpoint name ``Schema``)
-------------------------------------------

The schema service provides information regarding the range of records, record types, metadata elements and relationships configured for a CollectiveAccess system.

The ``tables`` query returns a list of valid primary table name, on which search, browse, edit and other record-oriented service operate:

.. code-block:: Text

	query {
		tables {
		  tables {
			name,
			code,
			types {
			  name,
			  code
			}
		  }
		}
	  }

The query returns a list of tables, with display names, codes and a list of available types for each.

The ``types`` query returns the same structure as the ``tables`` query, but for a single ``table``:

.. code-block:: Text

	  query {
		types(table: "ca_objects") {
			name,
			code,
			types {
			  name,
			  code
			}
		}
	  }

The response will be in the form:

.. code-block:: Text

	{
		"ok": true,
		"data": {
			"types": {
				"name": "objects",
				"code": "ca_objects",
				"types": [
					{
						"name": "Archival/Documentation",
						"code": "archival_item"
					},
					{
						"name": "Artifacts",
						"code": "artifact_item"
					},
					{
						"name": "Ebot",
						"code": "ebot_item"
					},
					{
						"name": "Ethnographic",
						"code": "ethno_item"
					},
					{
						"name": "Faunal",
						"code": "faunal_item"
					},
					{
						"name": "Images",
						"code": "image_item"
					},
					{
						"name": "Osteology",
						"code": "osteology_item"
					}
				]
			}
		}
	}

The ``bundles`` query will return a list of all available data bundles, including codes, settings, help text and type restrictions for a given table, and optionally, a type:

.. code-block:: Text

	  query {
		bundles(table: "ca_objects", type: "ebot_item") {
		  bundles {
			name,
			code, 
			description, 
			type, 
			dataType, 
			list, 
			typeRestrictions { 
				name, 
				type, 
				minAttributesPerRow, 
				maxAttributesPerRow
			}, 
			settings {
				name, 
				value
			}, 
			subelements { 
				name, 
				code, 
				type, 
				dataType, 
				list, 
				settings { 
					name, 
					value
				}
			}
		  }
		}
	  }

This query can be used to discover queryable and editable data elements from any record type in the CollectiveAccess system.

The ``relationshipTypes`` query will return a list of available relationship types for a relationship between two tables. To get relationship types for
relationships between two tables:

.. code-block:: Text

	query {
		  relationshipTypes(table: "ca_objects", relatedTable: "ca_entities") {
			relationshipTable,
			types {
				  id, code,
				  name, name_reverse, 
				  description, description_reverse,
				  rank, locale, isDefault,
				  restrictToTypeLeft, restrictToTypeRight, includeSubtypesLeft, includeSubtypesRight

			}
		  }
	}

The name of the relationship table may also be used. This query is equivalent to the previous one:

.. code-block:: Text

	query {
		  relationshipTypes(table: "ca_objects_x_entities") {
			relationshipTable,
			types {
				  id, code,
				  name, name_reverse, 
				  description, description_reverse,
				  rank, locale, isDefault,
				  restrictToTypeLeft, restrictToTypeRight, includeSubtypesLeft, includeSubtypesRight

			}
		  }
	}

The relationship type list returned is in the form:

.. code-block:: Text

	{
		"ok": true,
		"data": {
			"relationshipTypes": {
				"relationshipTable": "ca_objects_x_entities",
				"types": [
					{
						"name": "Artist",
						"name_reverse": "Artist",
						"description": "",
						"description_reverse": "",
						"code": "artist",
						"rank": 10,
						"locale": "en_US",
						"isDefault": false,
						"restrictToTypeLeft": null,
						"restrictToTypeRight": null,
						"includeSubtypesLeft": false,
						"includeSubtypesRight": false
					},
					{
						"name": "Illustrator",
						"name_reverse": "illustrator",
						"description": "",
						"description_reverse": "",
						"code": "artist_illustrator",
						"rank": 10,
						"locale": "en_US",
						"isDefault": false,
						"restrictToTypeLeft": null,
						"restrictToTypeRight": null,
						"includeSubtypesLeft": false,
						"includeSubtypesRight": false
					},
					{
						"name": "Cover artist",
						"name_reverse": "Cover artist",
						"description": "",
						"description_reverse": "",
						"code": "artist_cover_art",
						"rank": 10,
						"locale": "en_US",
						"isDefault": false,
						"restrictToTypeLeft": null,
						"restrictToTypeRight": null,
						"includeSubtypesLeft": false,
						"includeSubtypesRight": false
					}
				]
			}
		}
	}
	
The ``validateBundleCodes`` query provides a means to validate one or more bundle codes for a given table. Parameters include ``table`` and ``bundles`` (a list of bundle codes). The ``type`` parameter optionally restricts validation to the specified type. A typical query:

.. code-block:: Text

	query {
		  validateBundleCodes(table: "ca_objects", bundles: ["ca_objects.preferred_labels", "ca_objects.a_bad_code", "ca_objects.idno"]) {
			bundles {name, isValid}
		  }
	}
	
would return:

.. code-block:: Text

	{
		"ok": true,
		"data": {
			"validateBundleCodes": {
				"bundles": [
					{
						"name": "preferred_labels",
						"isValid": true
					},
					{
						"name": "a_bad_code",
						"isValid": false
					},
					{
						"name": "idno",
						"isValid": true
					}
				]
			}
		}
	}