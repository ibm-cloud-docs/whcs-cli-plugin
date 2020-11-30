---

copyright:
  years: 2020
lastupdated: "2020-11-23"

subcollection: whcs-cli-plugin
keywords: whcs annotator for clinical data CLI, whcs acd CLI, whcs annotator for clinical data command line, whcs acd command line, whcs annotator for clinical data terminal, whcs acd terminal, whcs annotator for clinical data shell, whcs acd shell

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:preview: .preview}
{:beta: .beta}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# WHCS Annotator for Clinical Data CLI (ibmcloud whcs acd)
{: #whcs-annotator-for-clinical-data-cli}

The Watson Health Cognitive Services (WHCS) Annotator for Clinical Data (ACD) CLI provides methods your application can use to customize the ACD annotators. These annotators can then be leveraged to analyze your unstructured text for the presence of medical codes, medical concepts, and contextual information.
{:shortdesc}


## Prerequisites
{: #whcs-annotator-for-clinical-data-cli-prereq}


* Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started).
* Install the <CLI_name> CLI by running the following command:

   ```sh
   ibmcloud plugin install whcs
   ```
   {: pre}

You're notified on the command line when updates to the {{site.data.keyword.cloud_notm}} CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use the latest commands. You can view the current version of all installed plug-ins by running `ibmcloud plugin list`.
{: tip}

## Profiles
{: #whcs-profiles-cli}
Manage persisted profiles.  A profile contains annotator configuration information that will be applied to the annotators specified in the annotator flow.

### ibmcloud whcs acd profiles
{: #whcs-cli-profiles-command}

Get a list of available persisted profiles.  Returns a summary including ID and description of the available persisted profiles.

```sh
ibmcloud whcs acd profiles
```

#### Examples
{: #whcs-profiles-cli-examples}

```sh
ibmcloud whcs acd profiles --output json
```
{: pre}

#### Example output
{: #whcs-profiles-cli-output}

```json
{
  "wh_acd.ibm_clinical_insights_v1.0_profile": {
    "description": "This cartridge produces a variety of clinical insights from clinical documents. The insights include medications, diagnosis, procedure history, diagnostic tests, abnormal labs, social and family history.",
    "publishedDate": "2020-10-19T19:42:58.480Z"
  },
  "wh_acd.ibm_covid-19_research_v1.0_profile": {
    "description": "Cartridge to support research into the 2019 Coronavirus",
    "publishedDate": "2020-08-26T14:14:03.577Z"
  }
}
```
{: screen}

### ibmcloud whcs acd profile-create
{: #whcs-cli-profile-create-command}

Persist a new profile.  A profile is identified by an ID.  This ID can optionally be specified as part of the request body when invoking `POST /v1/analyze` API.  A profile contains annotator configuration information that will be applied to the annotators specified in the annotator flow.<p>If a caller would choose to have the ID of the new profile generated on their behalf, then in the request body the "id" field of the profile definition should be an empty string ("").  The auto-generated ID would be a normalized form of the "name" field from the profile definition.<p><b>Sample Profile #1</b><br>A profile definition that configures the 'concept_detection' annotator to use the UMLS umls.latest library.<br><pre>{<br>  "id": "acd_profile_cd_umls_latest",<br>  "name": "Profile for the latest Concept Detection UMLS Library",<br>  "description": "Provides configurations for running Concept Detection with the latest UMLS library",<br>  "annotators": [<br>    {<br>      "name": "concept_detection",<br>      "parameters": {<br>         "libraries": ["umls.latest"]<br>       }<br>    }<br>  ]<br>}</pre><p><b>Sample Profile #2</b><br>A profile definition that configures the 'concept_detection' annotator to exclude any annotations where the semantic type does not equal 'neop'.<br><pre>{<br>  "id": "acd_profile_cd_neop_only",<br>  "name": "Profile for Concept Detection neop Semantic Type",<br>  "description": "Concept Detection configuration fitler to exclude annotations where semantic type does not equal 'neop'.",<br>  "annotators": [<br>    {<br>       "name": "concept_detection",<br>       "configurations": [<br>         {<br>           "filter": {<br>             "target": "unstructured.data.concepts",<br>             "condition": {<br>                "type": "match",<br>                "field": "semanticType",<br>                "values": [<br>                   "neop"<br>                 ],<br>                "not": false,<br>                "caseInsensitive": false,<br>                "operator": "equals"<br>              }<br>            }<br>         }<br>       ]<br>    }<br>  ]<br>}</pre>.

```sh
ibmcloud whcs acd profile-create [--id ID] [--name NAME] [--description DESCRIPTION] [--published-date PUBLISHED_DATE] [--publish PUBLISH] [--create-profile-version CREATE_PROFILE_VERSION] [--cartridge-id CARTRIDGE_ID] [--annotators ANNOTATORS]
```


#### Command options
{: #whcs-profile-create-cli-options}

<dl> 
<dt>--id (string)</dt>
<dd>Profile ID.</dd>
<dt>--name (string)</dt>
<dd>Profile name.</dd>
<dt>--description (string)</dt>
<dd>Profile description.</dd>
<dt>--published-date (string)</dt>
<dd>Profile publish date.</dd>
<dt>--publish (boolean)</dt>
<dd>Profile is published.</dd>
<dd>The default value is `false`.</dd>
<dt>--create-profile-version (string)</dt>
<dd>Profile version.</dd>
<dt>--cartridge-id (string)</dt>
<dd>Profile cartridge ID.</dd>
<dt>--annotators (string)</dt>
<dd>A string containing a JSON array of your annotator configuration.</dd>
</dl>

#### Examples
{: #whcs-profile-create-cli-examples}

```sh
ibmcloud whcs acd profile-create --id myProfile --annotators "[{\"name\":\"concept_detection\",\"parameters\":{\"libraries\":[\"umls.latest\"]}}]" --output json
```
{: pre}

#### Example output
{: #whcs-profile-create-cli-output}

```json
{
  "annotators": [
    {
      "name": "concept_detection",
      "parameters": {
        "libraries": [
          "umls.latest"
        ]
      }
    }
  ],
  "id": "myProfile"
}
```
{: screen}

### ibmcloud whcs acd profile
{: #whcs-cli-profile-command}

Get details of a specific profile.  Using the specified profile ID, retrieves the profile definition.

```sh
ibmcloud whcs acd profile --id ID
```


#### Command options
{: #whcs-profile-cli-options}

<dl> 
<dt>--id (string)</dt>
<dd>Profile ID. Required.</dd>
</dl>

#### Examples
{: #whcs-profile-cli-examples}

```sh
ibmcloud whcs acd profile --id wh_acd.ibm_clinical_insights_v1.0_profile --output json
```
{: pre}

#### Example output
{: #whcs-profile-cli-output}

```json
{
  "annotators": [
    {
      "name": "attribute_detection",
      "parameters": {
        "attribute_set": [
          "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_attributes_attribute_set"
        ]
      }
    },
    {
      "name": "lab_value",
      "parameters": {
        "filters": [
          "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_lab_value_filter_filter_content"
        ],
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "walking_assistance",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "spell_checker",
      "parameters": {
        "filters": [
          "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_spell_check_filter_filter"
        ]
      }
    },
    {
      "name": "allergy",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "medication",
      "parameters": {
        "filters": [
          "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_medication_filter_filter_content"
        ],
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "bathing_assistance",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "eating_assistance",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "section",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "configurations": [
        {}
      ],
      "name": "procedure",
      "parameters": {
        "filters": [
          "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_procedure_filter_filter_content"
        ],
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "ejection_fraction",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "model_broker",
      "parameters": {
        "models": [
          "medication",
          "diagnosis",
          "procedure",
          "normality"
        ]
      }
    },
    {
      "configurations": [
        {}
      ],
      "name": "symptom_disease",
      "parameters": {
        "filters": [
          "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_symptomdisease_filter_filter_content"
        ],
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "seeing_assistance",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "smoking",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "cancer",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "toileting_assistance",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "concept_detection",
      "parameters": {
        "filters": [
          "wh_acd.ibm_clinical_insights_v1.0_merged_concept_filter_concept_filter"
        ],
        "inference_rules": [
          "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_derived_concepts_concept_inference_rules"
        ],
        "libraries": [
          "wh_acd.ibm_clinical_insights_v1.0_-_normality_extension_library",
          "wh_acd.ibm_clinical_insights_v1.0_ibm_clinical_insights_umls_2020aa_library"
        ]
      }
    },
    {
      "name": "dressing_assistance",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    },
    {
      "name": "named_entities",
      "parameters": {
        "library": [
          "umls.2020AA"
        ]
      }
    }
  ],
  "description": "This cartridge produces a variety of clinical insights from clinical documents. The insights include medications, diagnosis, procedure history, diagnostic tests, abnormal labs, social and family history.",
  "id": "wh_acd.ibm_clinical_insights_v1.0_profile",
  "name": "IBM Clinical Insights (v1.0)",
  "publishedDate": "2020-10-19T19:42:58.480Z"
}
```
{: screen}

### ibmcloud whcs acd profile-update
{: #whcs-cli-profile-update-command}

Update a persisted profile.  Using the specified profile ID, update the profile definition.  This is a complete replacement of the existing profile definition using the JSON object provided in the request body.

```sh
ibmcloud whcs acd profile-update --id ID [--name NEW_NAME] [--description NEW_DESCRIPTION] [--published-date NEW_PUBLISHED_DATE] [--publish NEW_PUBLISH] [--version NEW_VERSION] [--cartridge-id NEW_CARTRIDGE_ID] [--annotators NEW_ANNOTATORS]
```


#### Command options
{: #whcs-profile-update-cli-options}

<dl> 
<dt>--id (string)</dt>
<dd>Profile ID. Required.</dd>
<dt>--name (string)</dt>
<dd>Profile name.</dd>
<dt>--description (string)</dt>
<dd>Profile description.</dd>
<dt>--published-date (string)</dt>
<dd>Profile publish date.</dd>
<dt>--publish (boolean)</dt>
<dd>Profile is published.</dd>
<dd>The default value is `false`.</dd>
<dt>--version (string)</dt>
<dd>Profile version.</dd>
<dt>--cartridge-id (string)</dt>
<dd>Profile cartridge ID.</dd>
<dt>--annotators (string)</dt>
<dd>A string containing a JSON array of your annotator configuration.</dd>
</dl>

#### Examples
{: #whcs-profile-update-cli-examples}

```sh
ibmcloud whcs acd profile-update --id myProfile --annotators "[{\"name\":\"concept_detection\",\"parameters\":{\"libraries\":[\"umls.latest\"]}},{\"name\":\"negation\"}]" --description "This is my profile" --output json
```
{: pre}

#### Example output
{: #whcs-profile-update-cli-output}

```json
{
  "annotators": [
    {
      "name": "concept_detection",
      "parameters": {
        "libraries": [
          "umls.latest"
        ]
      }
    },
    {
      "name": "negation"
    }
  ],
  "description": "This is my profile",
  "id": "myProfile"
}
```
{: screen}

### ibmcloud whcs acd profile-delete
{: #whcs-cli-profile-delete-command}

Delete a persisted profile.  Using the specified profile ID, delete the profile from the list of persisted profiles.

```sh
ibmcloud whcs acd profile-delete --id ID
```

#### Command options
{: #whcs-profile-delete-cli-options}

<dl> 
<dt>--id (string)</dt>
<dd>Profile ID. Required.</dd>
</dl>

#### Examples
{: #whcs-profile-delete-cli-examples}

```sh
ibmcloud whcs acd profile-delete --id myProfile --output json
```
{: pre}

## Flows
{: #whcs-flows-cli}
Manage persisted flows (also know as annotator flows).  A flow definition contains a list of one or more annotators and optionally can include annotator configuration and/or flow sequence.

### ibmcloud whcs acd flows
{: #whcs-cli-flows-command}

Get a list of available persisted flows.  Returns a summary including ID and description of the available persisted flows.

```sh
ibmcloud whcs acd flows
```

#### Examples
{: #whcs-flows-cli-examples}

```sh
ibmcloud whcs acd flows --output json
```
{: pre}

#### Example output
{: #whcs-flows-cli-output}

```json
{
  "wh_acd.ibm_clinical_insights_v1.0_standard_flow": {
    "description": "Default annotator flow for the insight cartridge",
    "publishedDate": "2020-10-19T19:42:58.480Z"
  },
  "wh_acd.ibm_covid-19_research_v1.0_covid-19_flow_flow": {
    "publishedDate": "2020-08-26T14:14:03.577Z"
  }
}
```
{: screen}

### ibmcloud whcs acd flow-create
{: #whcs-cli-flow-create-command}

Persist a new flow.  A flow is identified by an ID.  This ID can be specified when invoking `POST /v1/analyze/{flowID}` API.  A flow definition contains a list of one or more annotators and optionally can include annotator configuration and/or flow sequence.<p>If a caller would choose to have the ID of the new flow generated on their behalf, then in the request body the "id" field of the flow definition should be an empty string ("").  The auto-generated ID would be a normalized form of the "name" field from the flow definition.<p><p><b>Sample Flow #1</b><br>A flow definition that includes two annotators.<br><pre>{<br>  "id": "flow_simple",<br>  "name": "flow simple",<br>  "description": "A simple flow with two annotators",<br>  "annotatorFlows": [<br>      {<br>       "flow": {<br>          "elements": [<br>             {<br>               "annotator": {<br>                   "name": "concept_detection"<br>                }<br>             },<br>             {<br>               "annotator": {<br>                   "name": "symptom_disease"<br>                }<br>             }<br>           ],<br>       "async": false<br>        }<br>      }<br>   ]<br>}</pre><p><b>Sample Flow #2</b><br>A flow definition that includes the 'concept_detection' annotator and configuration details for the 'concept_detection' annotator.<br><pre>{<br>  "id": "flow_concept_detection_exclude_non_neop",<br>  "name": "flow concept detection exclude non neop",<br>  "description": "A flow excluding detected concepts that do not have 'neop' semantic type",<br>  "annotatorFlows": [<br>      {<br>       "flow": {<br>          "elements": [<br>             {<br>               "annotator": {<br>                   "name": "concept_detection",<br>                   "configurations": [<br>                      {<br>                        "filter": {<br>                           "target": "unstructured.data.concepts",<br>                           "condition": {<br>                              "type": "match",<br>                              "field": "semanticType",<br>                              "values": [<br>                                 "neop"<br>                                ],<br>                              "not": false,<br>                              "caseInsensitive": false,<br>                              "operator": "equals"<br>                            }<br>                         }<br>                      }<br>                    ]<br>                 }<br>              }<br>         ],<br>       "async": false<br>        }<br>      }<br>   ]<br>}</pre>.

```sh
ibmcloud whcs acd flow-create [--id ID] [--name NAME] [--description DESCRIPTION] [--published-date PUBLISHED_DATE] [--publish PUBLISH] [--create-flow-version CREATE_FLOW_VERSION] [--cartridge-id CARTRIDGE_ID] [--annotator-flows ANNOTATOR_FLOWS]
```


#### Command options
{: #whcs-flow-create-cli-options}

<dl> 
<dt>--id (string)</dt>
<dd>Flow ID.</dd>
<dt>--name (string)</dt>
<dd>Flow name.</dd>
<dt>--description (string)</dt>
<dd>Flow description.</dd>
<dt>--published-date (string)</dt>
<dd>Flow publish date.</dd>
<dt>--publish (boolean)</dt>
<dd>Flow is published.</dd>
<dd>The default value is `false`.</dd>
<dt>--create-flow-version (string)</dt>
<dd>Flow version.</dd>
<dt>--cartridge-id (string)</dt>
<dd>Flow cartridge ID.</dd>
<dt>--annotator-flows (string)</dt>
<dd>A string containing a JSON array of your annotator flow definition.</dd>
</dl>

#### Examples
{: #whcs-flow-create-cli-examples}

```sh
ibmcloud whcs acd flow-create --id myFlow --annotator-flows "[{\"flow\":{\"elements\":[{\"annotator\":{\"name\":\"concept_detection\"}}],\"async\":false}}]" --output json
```
{: pre}

#### Example output
{: #whcs-flow-create-cli-output}

```json
{
  "annotatorFlows": [
    {
      "flow": {
        "async": false,
        "elements": [
          {
            "annotator": {
              "name": "concept_detection"
            }
          }
        ]
      }
    }
  ],
  "id": "myFlow"
}
```
{: screen}

### ibmcloud whcs acd flow
{: #whcs-cli-flow-command}

Get details of a specific flow.  Using the specified flow ID, retrieves the flow definition.

```sh
ibmcloud whcs acd flow --id ID
```


#### Command options
{: #whcs-flow-cli-options}

<dl> 
<dt>--id (string)</dt>
<dd>Flow ID. Required.</dd>
</dl>

#### Examples
{: #whcs-flow-cli-examples}

```sh
ibmcloud whcs acd flow --id wh_acd.ibm_clinical_insights_v1.0_standard_flow --output json
```
{: pre}

#### Example output
{: #whcs-flow-cli-output}

```json
{
  "annotatorFlows": [
    {
      "flow": {
        "async": false,
        "elements": [
          {
            "annotator": {
              "name": "spell_checker",
              "parameters": {
                "apply_spell_corrections": [
                  "true"
                ],
                "spell_check_profile": [
                  "default"
                ]
              }
            }
          },
          {
            "annotator": {
              "name": "section"
            }
          },
          {
            "annotator": {
              "name": "concept_detection",
              "parameters": {
                "apply_spell_check": [
                  "true"
                ],
                "expanded": [
                  "true"
                ],
                "include_optional_fields": [
                  "medical_codes",
                  "source_vocabularies"
                ],
                "multiword_boundary": [
                  "true"
                ]
              }
            }
          },
          {
            "annotator": {
              "name": "lab_value"
            }
          },
          {
            "annotator": {
              "name": "medication"
            }
          },
          {
            "annotator": {
              "name": "procedure",
              "parameters": {
                "annotate_trigger_only": [
                  "true"
                ]
              }
            }
          },
          {
            "annotator": {
              "name": "symptom_disease",
              "parameters": {
                "annotate_trigger_only": [
                  "true"
                ]
              }
            }
          },
          {
            "annotator": {
              "name": "disambiguation",
              "parameters": {
                "disambiguation_profile": [
                  "clinical"
                ]
              }
            }
          },
          {
            "annotator": {
              "name": "concept_value"
            }
          },
          {
            "annotator": {
              "name": "negation"
            }
          },
          {
            "annotator": {
              "name": "model_broker"
            }
          },
          {
            "annotator": {
              "name": "attribute_detection",
              "parameters": {
                "detect_qualifiers": [
                  "false"
                ],
                "include_optional_fields": [
                  "medical_codes"
                ]
              }
            }
          }
        ]
      },
      "profile": "wh_acd.ibm_clinical_insights_v1.0_profile"
    }
  ],
  "description": "Default annotator flow for the insight cartridge",
  "id": "wh_acd.ibm_clinical_insights_v1.0_standard_flow",
  "name": "Standard (v1.0)",
  "publishedDate": "2020-10-19T19:42:58.480Z"
}
```
{: screen}

### ibmcloud whcs acd flow-update
{: #whcs-cli-flow-update-command}

Update a persisted flow.  Using the specified flow ID, update the persisted flow definition.  This is a complete replacement of the existing flow definition using the JSON object provided in the request body.

```sh
ibmcloud whcs acd flow-update --id ID [--name NEW_NAME] [--description NEW_DESCRIPTION] [--published-date NEW_PUBLISHED_DATE] [--publish NEW_PUBLISH] [--version NEW_VERSION] [--cartridge-id NEW_CARTRIDGE_ID] [--annotator-flows NEW_ANNOTATOR_FLOWS]
```


#### Command options
{: #whcs-flow-update-cli-options}

<dl> 
<dt>--id (string)</dt>
<dd>Flow ID. Required.</dd>
<dt>--name (string)</dt>
<dd>Flow name.</dd>
<dt>--description (string)</dt>
<dd>Flow description.</dd>
<dt>--published-date (string)</dt>
<dd>Flow publish date.</dd>
<dt>--publish (boolean)</dt>
<dd>Flow is published.</dd>
<dd>The default value is `false`.</dd>
<dt>--version (string)</dt>
<dd>Flow version.</dd>
<dt>--cartridge-id (string)</dt>
<dd>Flow cartridge ID.</dd>
<dt>--annotator-flows (string)</dt>
<dd>A string containing a JSON array of your annotator flow definition.</dd>
</dl>

#### Examples
{: #whcs-flow-update-cli-examples}

```sh
ibmcloud whcs acd flow-update --id myFlow --annotator-flows "[{\"flow\":{\"elements\":[{\"annotator\":{\"name\":\"concept_detection\",\"parameters\":{\"include_optional_fields\":[\"medical_codes\"]}}},{\"annotator\":{\"name\":\"disambiguation\"}}],\"async\":false}}]" --output json
```
{: pre}

#### Example output
{: #whcs-flow-update-cli-output}

```json
{
  "annotatorFlows": [
    {
      "flow": {
        "async": false,
        "elements": [
          {
            "annotator": {
              "name": "concept_detection",
              "parameters": {
                "include_optional_fields": [
                  "medical_codes"
                ]
              }
            }
          },
          {
            "annotator": {
              "name": "disambiguation"
            }
          }
        ]
      }
    }
  ],
  "id": "myFlow"
}
```
{: screen}

### ibmcloud whcs acd flow-delete
{: #whcs-cli-flow-delete-command}

Delete a persisted flow.  Using the specified flow ID, delete the flow from the list of persisted flows.

```sh
ibmcloud whcs acd flow-delete --id ID
```

#### Command options
{: #whcs-flow-delete-cli-options}

<dl> 
<dt>--id (string)</dt>
<dd>Flow ID. Required.</dd>
</dl>

#### Examples
{: #whcs-flow-delete-cli-examples}

```sh
ibmcloud whcs acd flow-delete --id myFlow --output json
```
{: pre}

## ACD
{: #whcs-acd-cli}
Detect entities & relations from unstructured data. Get details about the available ACD annotators.

### ibmcloud whcs acd analyze
{: #whcs-cli-analyze-command}

Detect entities and relations from unstructured data.  This API accepts a JSON request model featuring both the unstructured data to be analyzed as well as the desired annotator flow definition.<p><b>Annotator Chaining</b><br/>Sample request invoking both the concept_detection and symptom_disease annotators asynchronously. This sample request references configurations via a profile id. Profiles define configurations that can be referenced within a request. Profile is optional. A default profile is used if no profile id is available in the annotator flow. The default profile contains the parameters for the concept detection and the attribute detection. An empty profile can be used if absolutely no parameters are attached to any annotators. See [documentation](https://cloud.ibm.com/docs/wh-acd) for more information. </p><pre>{<br/>  "annotatorFlows": [<br/>    {<br/>      "profile" : "default_profile_v1.0", <br/>      "flow": {<br/>        "elements": [<br/>          {<br/>            "annotator": {<br/>              "name": "concept_detection"<br/>            }<br/>          },<br/>          {<br/>            "annotator": {<br/>              "name": "symptom_disease"<br/>             }<br/>          }<br/>        ],<br/>        "async": false<br/>      }<br/>    }<br/>  ],<br/>  "unstructured": [<br/>    {<br/>      "text": "Patient has lung cancer, but did not smoke. She may consider chemotherapy as part of a treatment plan."<br/>    }<br/>  ]<br/>}<br/></pre><p><b>Annotation Filtering</b><br/>Sample request invoking concept_detection with a filter defined to exclude any annotations detected from concept_detection where the semanticType field does not equal "neop".</p><pre>{<br/>  "annotatorFlows": [<br/>    {<br/>      "flow": {<br/>        "elements": [<br/>          {<br/>            "annotator": {<br/>              "name": "concept_detection",<br/>              "configurations": [<br/>                {<br/>                  "filter": {<br/>                     "target": "unstructured.data.concepts",<br/>                     "condition": {<br/>                        "type": "match",<br/>                        "field": "semanticType",<br/>                        "values": [<br/>                           "neop"<br/>                         ],<br/>                        "not": false,<br/>                        "caseInsensitive": false,<br/>                        "operator": "equals"<br/>                     }<br/>                  }<br/>                }<br/>              ]<br/>            }<br/>          }<br/>        ],<br/>       "async": false<br/>      }<br/>    }<br/>  ],<br/>  "unstructured": [<br/>    {<br/>      "text": "Patient has lung cancer, but did not smoke. She may consider chemotherapy as part of a treatment plan."<br/>    }<br/>  ]<br/>}<br/></pre><p><b>Annotators that support annotation filtering:</b> allergy, bathing_assistance, cancer, concept_detection, dressing_assistance, eating_assistance, ejection_fraction, lab_value, medication, named_entities, procedure, seeing_assistance, smoking, symptom_disease, toileting_assistance, walking_assistance.</p><hr/><p><b>Annotation Augmentation</b><br/>Sample request invoking the cancer annotator and providing a whitelist entry for a new custom surface form: "lungcancer".</p><pre>{<br/> "annotatorFlows": [<br/>    {<br/>     "flow": {<br/>       "elements": [<br/>          {<br/>           "annotator": {<br/>             "name": "cancer",<br/>             "configurations": [<br/>                {<br/>                 "whitelist": {<br/>                   "name": "cancer",<br/>                   "entries": [<br/>                      {<br/>                  "surfaceForms": [<br/>                   "lungcancer"<br/>                ],<br/>               "features": {<br/>                   "normalizedName": "lung cancer",<br/>                   "hccCode": "9",<br/>                   "icd10Code": "C34.9",<br/>                   "ccsCode": "19",<br/>                   "icd9Code": "162.9",<br/>                   "conceptId": "93880001"<br/>                }<br/>                      }<br/>                    ]<br/>                  }<br/>                }<br/>              ]<br/>            }<br/>          }<br/>        ],<br/>       "async": false<br/>      }<br/>    }<br/>  ],<br/> "unstructured": [<br/>    {<br/>     "text": "The patient was diagnosed with lungcancer, on Dec 23, 2011."<br/>    }<br/>  ]<br/>}<br/></pre><b>Annotators that support annotation augmentation:</b> allergy, bathing_assistance, cancer, dressing_assistance, eating_assistance, ejection_fraction, lab_value, medication, named_entities, procedure, seeing_assistance, smoking, symptom_disease, toileting_assistance, walking_assistance.<br/>.

```sh
ibmcloud whcs acd analyze [--unstructured UNSTRUCTURED] [--annotator-flows ANNOTATOR_FLOWS] [--debug-text-restore DEBUG_TEXT_RESTORE] [--return-analyzed-text RETURN_ANALYZED_TEXT]
```


#### Command options
{: #whcs-analyze-cli-options}

<dl> 
<dt>--unstructured (string)</dt>
<dd>A string containing a JSON array of one or more unstructured text elements to analyze.</dd>
<dt>--annotator-flows (string)</dt>
<dd>A string containing a JSON array of your annotator flow definition.</dd>
<dt>--debug-text-restore (boolean)</dt>
<dd>If true, any ReplaceTextChange annotations will be left in the container and the modified text, before restoring to original form, will be returned in the metadata.  Otherwise, these annotations and modified text will be removed from the container.</dd>
<dd>The default value is `false`.</dd>
<dt>--return-analyzed-text (boolean)</dt>
<dd>Set this to true to show the analyzed text in the response.</dd>
<dd>The default value is `false`.</dd>
</dl>

#### Examples
{: #whcs-analyze-cli-examples}

```sh
ibmcloud whcs acd analyze --annotator-flows "[{\"flow\":{\"elements\":[{\"annotator\":{\"name\":\"concept_detection\"}}],\"async\":false}}]" --unstructured "[{\"text\":\"Patient has lung cancer, but did not smoke.\"}]" --output json
```
{: pre}

#### Example output
{: #whcs-analyze-cli-output}

```json
{
  "annotatorFlows": [
    {
      "flow": {
        "async": false,
        "elements": [
          {
            "annotator": {
              "name": "concept_detection"
            }
          }
        ]
      }
    }
  ],
  "unstructured": [
    {
      "data": {
        "concepts": [
          {
            "begin": 0,
            "coveredText": "Patient has",
            "cui": "C0332310",
            "end": 11,
            "preferredName": "Has patient",
            "semanticType": "ftcn",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.FunctionalConcept"
          },
          {
            "begin": 12,
            "coveredText": "lung cancer",
            "cui": "C1306460",
            "end": 23,
            "preferredName": "Primary malignant neoplasm of lung",
            "semanticType": "neop",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.NeoplasticProcess"
          },
          {
            "begin": 12,
            "coveredText": "lung cancer",
            "cui": "C0684249",
            "end": 23,
            "preferredName": "Carcinoma of lung",
            "semanticType": "neop",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.NeoplasticProcess"
          },
          {
            "begin": 12,
            "coveredText": "lung cancer",
            "cui": "C0242379",
            "end": 23,
            "preferredName": "Malignant neoplasm of lung",
            "semanticType": "neop",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.NeoplasticProcess"
          },
          {
            "begin": 29,
            "coveredText": "did",
            "cui": "C1272695",
            "end": 32,
            "preferredName": "Done (qualifier value)",
            "semanticType": "qlco",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.QualitativeConcept"
          },
          {
            "begin": 29,
            "coveredText": "did",
            "cui": "C2828361",
            "end": 32,
            "preferredName": "Do (activity)",
            "semanticType": "acty",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.Activity"
          },
          {
            "begin": 33,
            "coveredText": "not",
            "cui": "C1518422",
            "end": 36,
            "preferredName": "Negation",
            "semanticType": "ftcn",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.FunctionalConcept"
          },
          {
            "begin": 37,
            "coveredText": "smoke",
            "cui": "C0037366",
            "end": 42,
            "preferredName": "Smoke",
            "semanticType": "hops",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.HazardousOrPoisonousSubstance"
          },
          {
            "begin": 37,
            "coveredText": "smoke",
            "cui": "C0037369",
            "end": 42,
            "preferredName": "Smoking",
            "semanticType": "inbe",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.IndividualBehavior"
          },
          {
            "begin": 37,
            "coveredText": "smoke",
            "cui": "C0439994",
            "end": 42,
            "preferredName": "Tobacco smoke",
            "semanticType": "hops",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.HazardousOrPoisonousSubstance"
          },
          {
            "begin": 37,
            "coveredText": "smoke",
            "cui": "C0453996",
            "end": 42,
            "preferredName": "Tobacco smoking behavior",
            "semanticType": "inbe",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.IndividualBehavior"
          },
          {
            "begin": 37,
            "coveredText": "smoke",
            "cui": "C1548578",
            "end": 42,
            "preferredName": "Location characteristic ID - Smoking",
            "semanticType": "inpr",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.IntellectualProduct"
          },
          {
            "begin": 37,
            "coveredText": "smoke",
            "cui": "C1881674",
            "end": 42,
            "preferredName": "Medical Device Emits Smoke",
            "semanticType": "fndg",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.Finding"
          }
        ]
      }
    }
  ]
}
```
{: screen}

### ibmcloud whcs acd analyze-with-flow
{: #whcs-cli-analyze-with-flow-command}

Detect entities and relations from unstructured text using a persisted flow. This API accepts the unstructured data to be analyzed and the ID of a persisted flow definition.
<p>This API accepts a flow identifier as well as a <emph>TEXT</emph> or a <emph>JSON</emph> request model featuring the unstructured text to be analyzed. <p/><p><b>JSON request model with unstructured text </b></p><pre>{<br/>  "unstructured": [<br/>    {<br/>      "text": "Patient has lung cancer, but did not smoke. She may consider chemotherapy as part of a treatment plan."<br/>    }<br/>  ]<br/>}<br/></pre><p><b>JSON request model with existing annotations </b><br/></p><pre>{<br> "unstructured": [<br>    {<br>      "text": "Patient will not start on cisplatin 80mg on 1/1/2018. Patient is also diabetic.",<br>      "data": {<br>        "concepts": [<br>          {<br>            "cui": "C0030705",<br>            "preferredName": "Patients",<br>            "semanticType": "podg",<br>            "source": "umls",<br>            "sourceVersion": "2017AA",<br>            "type": "umls.PatientOrDisabledGroup",<br>            "begin": 0,<br>            "end": 7,<br>            "coveredText": "Patient"<br>          }<br> ]<br>      }  <br>    } <br> ]<br>}<br></pre>.

```sh
ibmcloud whcs acd analyze-with-flow --flow-id FLOW_ID --return-analyzed-text RETURN_ANALYZED_TEXT [--body BODY] [--content-type CONTENT_TYPE] [--debug-text-restore DEBUG_TEXT_RESTORE]
```


#### Command options
{: #whcs-analyze-with-flow-cli-options}

<dl> 
<dt>--flow-id (string)</dt>
<dd>Flow ID. Required.</dd>
<dt>--return-analyzed-text (boolean)</dt>
<dd>Set this to true to show the analyzed text in the response. Required.</dd>
<dd>The default value is `false`.</dd>
<dt>--body (string)</dt>
<dd>Input request data in TEXT or JSON format.</dd>
<dt>--content-type (string)</dt>
<dd>Reflects the type of data provided in the `body` command option.</dd>
<dd>Allowable values: `application/json` or `text/plain`.</dd>
<dt>--debug-text-restore (boolean)</dt>
<dd>If true, any ReplaceTextChange annotations will be left in the container and the modified text, before restoring to original form, will be returned in the metadata.  Otherwise, these annotations and modified text will be removed from the container.</dd>
<dd>The default value is `false`.</dd>
</dl>

#### Examples
{: #whcs-analyze-with-flow-cli-examples}

```sh
ibmcloud whcs acd analyze-with-flow --body "Patient has lung cancer but did not smoke" --content-type "text/plain" --flow-id "wh_acd.ibm_clinical_insights_v1.0_standard_flow" --return-analyzed-text "false" --output json
```
{: pre}

#### Example output
{: #whcs-analyze-with-flow-cli-output}

```json
{
  "unstructured": [
    {
      "data": {
        "SymptomDiseaseInd": [
          {
            "begin": 12,
            "ccsCode": "19",
            "coveredText": "lung cancer",
            "cui": "C1306460",
            "dateInMilliseconds": " ",
            "disambiguationData": {
              "validity": "VALID"
            },
            "end": 23,
            "hccCode": "9",
            "icd10Code": "C34.90,C34.9",
            "icd9Code": "162.9",
            "insightModelData": {
              "diagnosis": {
                "familyHistoryScore": 0,
                "suspectedScore": 0.008,
                "symptomScore": 0.001,
                "traumaScore": 0,
                "usage": {
                  "discussedScore": 0.63,
                  "explicitScore": 0.37,
                  "patientReportedScore": 0
                }
              }
            },
            "modality": "positive",
            "negated": false,
            "snomedConceptId": "93880001",
            "symptomDieaseNormalizedName": "primary malignant neoplasm of lung",
            "type": "aci.SymptomDiseaseInd"
          }
        ],
        "concepts": [
          {
            "begin": 12,
            "coveredText": "lung cancer",
            "cui": "C1306460",
            "disambiguationData": {
              "validity": "NO_DECISION"
            },
            "end": 23,
            "icd10Code": "C34.9,C34.90",
            "icd9Code": "162.9",
            "insightModelData": {
              "diagnosis": {
                "familyHistoryScore": 0,
                "suspectedScore": 0.008,
                "symptomScore": 0.001,
                "traumaScore": 0,
                "usage": {
                  "discussedScore": 0.63,
                  "explicitScore": 0.37,
                  "patientReportedScore": 0
                }
              }
            },
            "negated": false,
            "preferredName": "Primary malignant neoplasm of lung",
            "semanticType": "neop",
            "snomedConceptId": "93880001",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.NeoplasticProcess",
            "uid": 2,
            "vocabs": "MTH,SNOMEDCT_US"
          },
          {
            "begin": 12,
            "coveredText": "lung cancer",
            "cui": "C0684249",
            "disambiguationData": {
              "validity": "NO_DECISION"
            },
            "end": 23,
            "nciCode": "C4878",
            "negated": false,
            "preferredName": "Carcinoma of lung",
            "semanticType": "neop",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.NeoplasticProcess",
            "vocabs": "MTH,CHV,NCI_CTRP,CST,NCI,OMIM,COSTAR,NCI_CTEP-SDC,PDQ,NCI_NCI-GLOSS,NCI_CPTAC"
          },
          {
            "begin": 12,
            "coveredText": "lung cancer",
            "cui": "C0242379",
            "disambiguationData": {
              "validity": "NO_DECISION"
            },
            "end": 23,
            "icd10Code": "C34.9,C34.90",
            "icd9Code": "162.9,162.8",
            "loincId": "LA15687-9",
            "meshId": "M0012750",
            "nciCode": "C7377",
            "negated": false,
            "preferredName": "Malignant neoplasm of lung",
            "semanticType": "neop",
            "snomedConceptId": "363358000",
            "source": "umls",
            "sourceVersion": "2020AA",
            "type": "umls.NeoplasticProcess",
            "vocabs": "MTH,LNC,CSP,MSH,HPO,OMIM,COSTAR,CHV,NCI_CTRP,MEDLINEPLUS,NCI,LCH_NW,AOD,SNOMEDCT_US,PDQ"
          },
          {
            "begin": 12,
            "coveredText": "lung cancer",
            "cui": "C1306460",
            "derivedFrom": [
              {
                "uid": 2
              }
            ],
            "disambiguationData": {
              "validity": "VALID"
            },
            "end": 23,
            "insightModelData": {
              "diagnosis": {
                "familyHistoryScore": 0,
                "suspectedScore": 0.008,
                "symptomScore": 0.001,
                "traumaScore": 0,
                "usage": {
                  "discussedScore": 0.63,
                  "explicitScore": 0.37,
                  "patientReportedScore": 0
                }
              }
            },
            "negated": false,
            "preferredName": "Primary malignant neoplasm of lung",
            "ruleId": "33e6be29-a746-40f1-a012-952410bade6f",
            "source": "Clinical Insights - Derived Concepts",
            "sourceVersion": "v1.0",
            "type": "ICDiagnosis"
          }
        ],
        "negatedSpans": [
          {
            "begin": 36,
            "coveredText": "smoke",
            "end": 41,
            "type": "NegatedSpan"
          }
        ],
        "spellCorrectedText": [
          {
            "CorrectedText": "Patient has lung cancer but did not smoke",
            "DebugText": ""
          }
        ]
      },
      "text": "Patient has lung cancer but did not smoke"
    }
  ]
}
```
{: screen}

### ibmcloud whcs acd annotators
{: #whcs-cli-annotators-command}

Get a list of available ACD annotators that can be leveraged to detect entities and relations from unstructured data. One or more ACD annotators can be leveraged within a single request to the service.

```sh
ibmcloud whcs acd annotators
```

#### Examples
{: #whcs-annotators-cli-examples}

```sh
ibmcloud whcs acd annotators --output json
```
{: pre}

#### Example output
{: #whcs-annotators-cli-output}

```json
{
  "allergy": {
    "description": "Detect allergy information from clinic notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "attribute_detection": {
    "description": "Detect clinical attributes from a set of concepts and concept values.",
    "version": "2020-11-09T06:03:59Z"
  },
  "bathing_assistance": {
    "description": "Detect the need for bathing assistance from case notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "cancer": {
    "description": "Detect cancer history \u0026 diagnoses from clinic notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "concept_detection": {
    "description": "Detect UMLS concepts from medical data.",
    "version": "2020-11-09T07:02:15Z"
  },
  "concept_value": {
    "description": "Detect values associated with a set of concepts.",
    "version": "2020-11-09T08:01:32Z"
  },
  "disambiguation": {
    "description": "Disambiguate medical concepts based on surrounding context.",
    "version": "2020-11-09T07:02:22Z"
  },
  "dressing_assistance": {
    "description": "Detect the need for dressing assistance from case notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "eating_assistance": {
    "description": "Detect the need for eating assistance from case notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "ejection_fraction": {
    "description": "Detect ejection fraction from clinic notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "hypothetical": {
    "description": "Detect spans of text deemed to be hypothetical in context such as 'An ultrasound guided biopsy is recommended'.",
    "version": "2020-11-09T08:01:40Z"
  },
  "lab_value": {
    "description": "Detect values from lab notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "medication": {
    "description": "Detect medications from clinic notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "model_broker": {
    "description": "Model broker service for the clinical insights services",
    "version": "2020-11-09T07:02:13Z"
  },
  "named_entities": {
    "description": "Detect named entities from clinic notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "negation": {
    "description": "Detect negated spans from medical data.",
    "version": "2020-11-09T09:01:53Z"
  },
  "procedure": {
    "description": "Detect procedures from clinic notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "relation": {
    "description": "Detect ontology-based relationships within clinical data.",
    "version": "2020-11-09T10:02:59Z"
  },
  "section": {
    "description": "Detect sections within the case notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "seeing_assistance": {
    "description": "Detect the need for seeing assistance from case notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "smoking": {
    "description": "Detect smoking history from case notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "spell_checker": {
    "description": "Spell check word and phrases in documents.",
    "version": "2020-11-09T10:03:01Z"
  },
  "symptom_disease": {
    "description": "Detect symptoms \u0026 diseases from clinic notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "toileting_assistance": {
    "description": "Detect the need for bathroom assistance from case notes.",
    "version": "2020-11-09T06:04:01Z"
  },
  "walking_assistance": {
    "description": "Detect the need for walking assistance from case notes.",
    "version": "2020-11-09T06:04:01Z"
  }
}
```
{: screen}

### ibmcloud whcs acd annotator
{: #whcs-cli-annotator-command}

Get details of an ACD annotator that can be used to detect entities and relations from unstructured data.

```sh
ibmcloud whcs acd annotator --id ID
```

#### Command options
{: #whcs-annotator-cli-options}

<dl> 
<dt>--id (string)</dt>
<dd>Annotator ID. Required.</dd>
</dl>

#### Examples
{: #whcs-annotator-cli-examples}

```sh
ibmcloud whcs acd annotator --id procedure --output json
```
{: pre}

#### Example output
{: #whcs-annotator-cli-output}

```json
{
  "description": "Detect procedures from clinic notes.",
  "version": "2020-11-09T06:04:01Z"
}
```
{: screen}

### ibmcloud whcs acd user-specific-artifacts-delete
{: #whcs-cli-user-specific-artifacts-delete-command}

Delete your tenant-specific artifacts from the Annotator for Clinical Data service instance.

```sh
ibmcloud whcs acd user-specific-artifacts-delete
```

#### Examples
{: #whcs-user-specific-artifacts-delete-cli-examples}

```sh
ibmcloud whcs acd user-specific-artifacts-delete --output json
```
{: pre}

## Cartridges
{: #whcs-cartridges-cli}
A cartridge contains annotator customization details.  It can be deployed to your Annotator for Clinical Data service instance and then leveraged when analyzing your unstructured data.

### ibmcloud whcs acd cartridges
{: #whcs-cli-cartridges-command}

Returns a summary including ID and status of the available cartridge deployments.

```sh
ibmcloud whcs acd cartridges
```

#### Examples
{: #whcs-cartridges-cli-examples}

```sh
ibmcloud whcs acd cartridges --output json
```
{: pre}

#### Example output
{: #whcs-cartridges-cli-output}

```json
{
  "cartridges": [
    {
      "artifactResponseCode": 200,
      "correlationId": "7039942c-49f0-4191-bba2-b7bbb8a9e337",
      "duration": "10",
      "endTime": "2020-11-11T19:29:10.920Z",
      "id": "",
      "name": "IBM Clinical Insights",
      "startTime": "2020-11-11T19:29:00.806Z",
      "status": "completed",
      "statusLocation": "api/v1/cartridges/wh_acd.ibm_clinical_insights_v1.0"
    },
    {
      "artifactResponseCode": 200,
      "correlationId": "f0541f33-7da8-456b-b54a-151bd9b43813",
      "duration": "15",
      "endTime": "2020-11-11T19:29:28.157Z",
      "id": "wh_acd.ibm_covid-19_research_v1.0",
      "name": "IBM COVID-19 Research",
      "startTime": "2020-11-11T19:29:12.745Z",
      "status": "completed",
      "statusLocation": "api/v1/cartridges/wh_acd.ibm_covid-19_research_v1.0"
    }
  ]
}
```
{: screen}

### ibmcloud whcs acd cartridge-create
{: #whcs-cli-cartridge-create-command}

Create a cartridge deployment from a cartridge archive file.

```sh
ibmcloud whcs acd cartridge-create [--archive-file ARCHIVE_FILE]
```


#### Command options
{: #whcs-cartridge-create-cli-options}

<dl> 
<dt>--archive-file (binary)</dt>
<dd>The cartridge archive file.</dd>
</dl>

#### Examples
{: #whcs-cartridge-create-cli-examples}

```sh
ibmcloud whcs acd cartridge-create --archive-file demo_cartridge_v1.0.zip --output json
```
{: pre}

#### Example output
{: #whcs-cartridge-create-cli-output}

```json
{
  "artifactResponseCode": 0,
  "correlationId": "d894ff10-af8b-4665-b1a2-db33ec8609a2",
  "flows": [
    "demo_cartridge_v1.0_default_flow"
  ],
  "id": "demo_cartridge_v1.0",
  "name": "Demo Cartridge",
  "profiles": [
    "demo_cartridge_v1.0_profile"
  ],
  "statusCode": 202,
  "statusLocation": "api/v1/cartridges/demo_cartridge_v1.0",
  "version": "v1.0"
}
```
{: screen}

### ibmcloud whcs acd cartridge-update
{: #whcs-cli-cartridge-update-command}

Update a previous cartridge deployment from a cartridge archive file.

```sh
ibmcloud whcs acd cartridge-update [--archive-file ARCHIVE_FILE]
```


#### Command options
{: #whcs-cartridge-update-cli-options}

<dl> 
<dt>--archive-file (binary)</dt>
<dd>The cartridge archive file.</dd>
</dl>

#### Examples
{: #whcs-cartridge-update-cli-examples}

```sh
ibmcloud whcs acd cartridge-update --archive-file demo_cartridge_v1.0.zip --output json
```
{: pre}

#### Example output
{: #whcs-cartridge-update-cli-output}

```json
{
  "artifactResponseCode": 0,
  "correlationId": "3280b8fa-0d21-4c12-af99-bfe97068982b",
  "flows": [
    "demo_cartridge_v1.0_default_flow"
  ],
  "id": "demo_cartridge_v1.0",
  "name": "Demo Cartridge",
  "profiles": [
    "demo_cartridge_v1.0_profile"
  ],
  "statusCode": 202,
  "statusLocation": "api/v1/cartridges/demo_cartridge_v1.0",
  "version": "v1.0"
}
```
{: screen}

### ibmcloud whcs acd cartridge
{: #whcs-cli-cartridge-command}

Get details of a specific cartridge deployment.  Using the specified cartridge ID, retrieves the deployment status.

```sh
ibmcloud whcs acd cartridge --id ID
```


#### Command options
{: #whcs-cartridge-cli-options}

<dl> 
<dt>--id (string)</dt>
<dd>The cartridge ID. Required.</dd>
</dl>

#### Examples
{: #whcs-cartridge-cli-examples}

```sh
ibmcloud whcs acd cartridge --id wh_acd.ibm_clinical_insights_v1.0 --output json
```
{: pre}

#### Example output
{: #whcs-cartridge-cli-output}

```json
{
  "artifactResponse": [
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_umls_extension_dictionary_dictionary",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_derived_concepts_concept_inference_rules",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_standard_flow",
      "code": 201,
      "description": "Flow wh_acd.ibm_clinical_insights_v1.0_standard_flow created.",
      "level": "INFO",
      "message": "Created"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_spell_check_filter_filter",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_symptom_disease_extension_dictionary",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_procedure_extension_dictionary",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_medication_filter_filter",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_procedure_filter_filter",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_attributes_attribute_set",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_normality_extension_dictionary",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_-_normality_extension_library",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_symptomdisease_filter_filter",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_clinical_insights_-_lab_value_filter_filter",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_merged_concept_filter_concept_filter",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_ibm_clinical_insights_umls_2020aa_library",
      "code": 200,
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "wh_acd.ibm_clinical_insights_v1.0_profile",
      "code": 200,
      "description": "Profile wh_acd.ibm_clinical_insights_v1.0_profile updated.",
      "level": "INFO",
      "message": "OK"
    }
  ],
  "artifactResponseCode": 200,
  "correlationId": "7039942c-49f0-4191-bba2-b7bbb8a9e337",
  "duration": "10",
  "endTime": "2020-11-11T19:29:10.920Z",
  "id": "wh_acd.ibm_clinical_insights_v1.0",
  "name": "IBM Clinical Insights",
  "startTime": "2020-11-11T19:29:00.806Z",
  "status": "completed",
  "statusLocation": "api/v1/cartridges/wh_acd.ibm_clinical_insights_v1.0"
}
```
{: screen}

### ibmcloud whcs acd cartridge-deploy
{: #whcs-cli-cartridge-deploy-command}

Deploy a cartridge from a cartridge archive file.

```sh
ibmcloud whcs acd cartridge-deploy [--archive-file ARCHIVE_FILE] [--update UPDATE]
```


#### Command options
{: #whcs-cartridge-deploy-cli-options}

<dl> 
<dt>--archive-file (binary)</dt>
<dd>The cartridge archive file.</dd>
<dt>--update (boolean)</dt>
<dd>Update a previous cartridge deployment if it already exists.</dd>
<dd>The default value is `false`.</dd>
</dl>

#### Examples
{: #whcs-cartridge-deploy-cli-examples}

```sh
ibmcloud whcs acd cartridge-deploy --archive-file demo_cartridge_v1.0.zip --update true --output json
```
{: pre}

#### Example output
{: #whcs-cartridge-deploy-cli-output}

```json
{
  "artifactResponse": [
    {
      "artifact": "demo_cartridge_v1.0_general_medical_literature_dictionary_dictionary",
      "code": 200,
      "correlationId": "55480277-b2f6-4374-8536-0813e4c2ec86",
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "demo_cartridge_v1.0_general_medical_literature_dictionary_library",
      "code": 200,
      "correlationId": "55480277-b2f6-4374-8536-0813e4c2ec86",
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "demo_cartridge_v1.0_general_medical_attribute_set",
      "code": 200,
      "correlationId": "55480277-b2f6-4374-8536-0813e4c2ec86",
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "demo_cartridge_v1.0_profile",
      "code": 200,
      "correlationId": "55480277-b2f6-4374-8536-0813e4c2ec86",
      "description": "Profile demo_cartridge_v1.0_profile updated.",
      "level": "INFO",
      "message": "OK"
    },
    {
      "artifact": "demo_cartridge_v1.0_default_flow",
      "code": 201,
      "correlationId": "55480277-b2f6-4374-8536-0813e4c2ec86",
      "description": "Flow demo_cartridge_v1.0_default_flow created.",
      "level": "INFO",
      "message": "Created"
    }
  ],
  "code": 200
}
```
{: screen}

## Status
{: #whcs-status-cli}
Get the health check status of this Annotator for Clinical Data service instance.

### ibmcloud whcs acd status
{: #whcs-cli-status-command}

Determine if service is running correctly.  This is a health check status of the service.  It will return a 500 error if the service state is not OK.

```sh
ibmcloud whcs acd status
```

#### Examples
{: #whcs-status-cli-examples}

```sh
ibmcloud whcs acd status --output json
```
{: pre}

#### Example output
{: #whcs-status-cli-output}

```json
{
  "serviceState": "OK"
}
```
{: screen}
