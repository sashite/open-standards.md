# Structured Hypermedia Language <small>Specification and Implementation Guide</small>

A general purpose <abbr title="JavaScript Object Notation">JSON</abbr>-based format for hypermedia.

<dl class="dl-horizontal">
  <dt>Created</dt>
  <dd><time datetime="2013-09-28T01:23:45Z">28 September 2013</time></dd>

  <dt>Updated</dt>
  <dd><time datetime="2013-10-18T23:42:34Z">18 October 2013</time></dd>

  <dt>Status</dt>
  <dd>alpha</dd>

  <dt>Author</dt>
  <dd><a rel="external" href="//twitter.com/cyri_">@cyri_</a></dd>
</dl>

<div class="alert alert-warning">
  <button type="button" class="close" data-dismiss="alert">&times;</button>
  <strong>This is a work in progress!</strong>
  This is an alpha document and may be updated at any time.
</div>

<div class="sub-title">Abstract</div>

This specification defines syntax for <q>Structured Hypermedia Language</q> (<abbr title="Structured Hypermedia Language">SHL</abbr>), a computer-processible <abbr title="JavaScript Object Notation">JSON</abbr>-based format for describing the resources' actions of applications.

<div class="sub-title">Status of this document</div>

This document is a beta version of the <abbr title="Structured Hypermedia Language">SHL</abbr> specification.

<div class="sub-title">Conformance requirements</div>

All diagrams, examples, and notes in this specification are non-normative, as are all sections explicitly marked non-normative. Everything else in this specification is normative.

The key words "MUST", "MUST NOT", "REQUIRED", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in the normative parts of this document are to be interpreted as described in [RFC2119](//tools.ietf.org/html/rfc2119).  The key word "OPTIONALLY" in the normative parts of this document is to be interpreted with the same normative meaning as "MAY" and "OPTIONAL". For readability, these words do not appear in all uppercase letters in this specification.

<div class="sub-title">Table of contents</div>

* [Media type](#vector)
* [Root](#root)
    * [Resource representation](#root-resource_representation)
        * [Properties](#root-resource_representation-properties)
* [Example](#example)
* [License](#license)

# Media type

<abbr title="Structured Hypermedia Language">SHL</abbr> is a little bit like <a rel="external" href="//www.sitemaps.org/">Sitemaps</a> for machines, in that it is generic and designed to drive many different types of application via hyperlinks.  The difference is that Sitemaps has features to inform search engines about pages, whereas <abbr title="Structured Hypermedia Language">SHL</abbr> is intended to inform crawlers about API's resources.

The media type for <abbr title="Structured Hypermedia Language">SHL</abbr> is `application/vnd.shl+json`.

# Root

## Resource representation

The <abbr title="JavaScript Object Notation">JSON</abbr> structure below shows the format of the resource:

    {
      {media_type}: {
        "resources": {resources},
        "version": {version}
      }
    }

### Properties

The following table defines the properties that appear in this resource:

<table class="table table-condensed">
  <thead>
    <tr>
      <th>Property name</th>
      <th>Value</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>media_type</code></td>
      <td>string</td>
      <td>
        Indicates a media type for <abbr title="JavaScript Object Notation">JSON</abbr> as a namespace.
        A version may also be present.
      </td>
    </tr>
    <tr>
      <td><code>resources</code></td>
      <td>hash</td>
      <td>
        Indicates the list of resources such as:
        <pre><code class="json">{
    "profile": {
        "GET": {},
        "POST": {
            "display_email": [
                "Boolean",
                {
                    "default": false
                }
            ],
            "email": [
                "String",
                {
                    "required?": true
                }
            ],
            "name": [
                "String",
                {
                    "required?": false
                }
            ]
        },
        "PATCH": {
            "display_email": [
                "Boolean",
                {
                    "default": false
                }
            ],
            "email": [
                "String",
                {
                    "required?": false
                }
            ],
            "name": [
                "String",
                {
                    "required?": false
                }
            ]
        }
    }
}</code></pre>
      </td>
    </tr>
    <tr>
      <td><code>version</code></td>
      <td>string</td>
      <td>
        Version number.
        The value is specified in <a rel="external" href="//semver.org/spec/v2.0.0.html">Semantic Versioning 2.0.0</a> format.
      </td>
    </tr>
  </tbody>
</table>

# Example

Description of resources.

    GET https://api.example.com/ HTTP/1.1

    {
        "application/vnd.example.api.shl+json; version=42.0.0": {
            "resources": {
                "/emails": {"/:email_id": {"/password": {"/:password_id": {"token": "GET": {}}}}},
                "games": {
                    "/games": {
                        "GET": {}
                    }
                },
                "game": {
                    "/games/:id": {
                        "GET": {}
                    },
                    "/me/challenges/:challenge_id/game": {
                        "DELETE": {},
                        "GET": {},
                        "PATCH": {
                            "display_email": [
                                "Boolean",
                                {
                                    "default": false
                                }
                            ],
                            "email": [
                                "String",
                                {
                                    "required?": false
                                }
                            ],
                            "name": [
                                "String",
                                {
                                    "required?": false
                                }
                            ]
                        },
                        "POST": {
                            "display_email": [
                                "Boolean",
                                {
                                    "default": false
                                }
                            ],
                            "email": [
                                "String",
                                {
                                    "required?": false
                                }
                            ],
                            "name": [
                                "String",
                                {
                                    "required?": false
                                }
                            ]
                        }
                    }
                },
                "/me": {
                    "DELETE": {},
                    "GET": {},
                    "PUT": {
                        "display_email": [
                            "Boolean",
                            {
                                "default": false
                            }
                        ],
                        "email": [
                            "String",
                            {
                                "required?": false
                            }
                        ],
                        "name": [
                            "String",
                            {
                                "required?": false
                            }
                        ]
                    },
                    "/challenges": {
                        "GET": {},
                        "/:id": {
                            "DELETE": {},
                            "GET": {}
                        }
                    },
                    "/opponents": {
                        "/:opponent_id": {
                            "/challenges": {
                                "POST": {}
                            }
                        }
                    }
                }
            },
            "move": {
                "/me/challenges/:challenge_id/game/moves": {
                    "POST": {
                        "display_email": [
                            "Boolean",
                            {
                                "default": false
                            }
                        ],
                        "email": [
                            "String",
                            {
                                "required?": false
                            }
                        ],
                        "name": [
                            "String",
                            {
                                "required?": false
                            }
                        ]
                    }
                }
            },
            "opponents": {
                "/me/opponents": {
                    "GET": {}
                }
            },
            "opponent": {
                "/me/opponents/:id": {
                    "GET": {}
                }
            },
            "password": {
                "/me/password": {
                    "PUT": {
                        "display_email": [
                            "Boolean",
                            {
                                "default": false
                            }
                        ],
                        "email": [
                            "String",
                            {
                                "required?": false
                            }
                        ],
                        "name": [
                            "String",
                            {
                                "required?": false
                            }
                        ]
                    }
                }
            }
        },
        "version": "1.0.0"
    }

# License

The Structured Hypermedia Language (<abbr title="Structured Hypermedia Language">SHL</abbr>) format is in the [Public Domain](//en.wikipedia.org/wiki/Public_Domain).

The format for <abbr title="Structured Hypermedia Language">SHL</abbr> is in the public domain and is thus free for use for any purpose, commercial or private.
