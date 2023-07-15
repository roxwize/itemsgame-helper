<script>
    import attribs from "./attribs.json";
    import descs from "./descs_en.json";
    const def = Object.entries(attribs);
    console.log(def);
    let attribArray = def;

    const FORMATS = {
        "value_is_percentage": "Percentage",
        "value_is_inverted_percentage": "Inverted Percentage",
        "value_is_additive": "Additive",
        "value_is_additive_percentage": "Additive Percentage",
        "value_is_or": "Toggle (1/0)",
        "value_is_date": "Date",
        "value_is_particle_index": "Particle Index",
        "value_is_account_id": "Account ID",
        "value_is_item_def": "Item Definition",
        "value_is_from_lookup_table": "Value From Lookup Table",
        "value_is_killstreakeffect_index": "Killstreak Effect Index",
        "value_is_killstreak_idleeffect_index": "Killstreak Idle Effect Index",
        "c_value_is_condition": "Condition",
        "": "None"
    }

    function onSearchUpdate(e) {
        attribArray = e.target.value.length > 0 ? def.filter(([k, v]) => v.name.includes(e.target.value) || k === parseInt(e.target.value)) : def;
    }
</script>

<div class="attrib-selector">
    <input type="text" placeholder="Search" id="attrib-search" on:input={onSearchUpdate} /><br />
    {#each attribArray as [i, attrib]}
        <div class="attribute" id="{i}">
            <div class="attrib-top {attrib.effect_type || 'neutral'}">
                <strong class="attrib-name">#{i}: {attrib.name}{#if attrib.hidden === "1"}&nbsp;[Hidden]{/if}</strong>
                &nbsp;<span class="attrib-format">{FORMATS[attrib.description_format || ""]}</span>
                &nbsp;<span class="attrib-class">({attrib.attribute_class || "None"})</span>
            </div>
            <div class="attrib-bottom">
                {#if attrib.description_string != undefined} <span class="attrib-text">"{#if attrib.description_string[0] == "#" && (descs[attrib.description_string.substring(1)])}{(descs[attrib.description_string.substring(1)]).replace(/\\n/g, "; ")}{:else}{attrib.description_string}{/if}"</span> {/if}
                {#if (attrib.description_format == "c_value_is_condition" || attrib.syntax !== undefined)}
                    <span class="attrib-syntax">Syntax: "{@html attrib.syntax || '&lt;condition id&gt; &lt;duration&gt;'}"</span>
                {/if}
                {#if attrib.notes}
                    <div class="attrib-notes">{@html attrib.notes}</div>
                {/if}
            </div>
        </div>
    {/each}
</div>